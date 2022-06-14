# Session & Token

# Session-flow

1. session을 설정해둡니다.

```jsx
// server/index.js

app.use(
  session({
    secret: "@codestates",
    resave: false,
    saveUninitialized: true,
    cookie: {
      domain: "localhost",
      path: "/",
      maxAge: 24 * 6 * 60 * 10000,
      sameSite: "none",
      httpOnly: true,
      secure: true,
    },
  })
);
```

1. client에서 로그인 요청(post)을 보냅니다.
    
    db에서 유저의 존재 유무를 확인하기 위해 data에 userId와 password를 담아 보냅니다. 그리고 users/userinfo호 get 요청을 보내어 session을 받습니다.
    

```jsx
// client/src/components/login.js

class Login extends Component {
  constructor(props) {
    super(props);
    this.state = {
      username: "",
      password: "",
    };
    this.inputHandler = this.inputHandler.bind(this);
    this.loginRequestHandler = this.loginRequestHandler.bind(this);
  }

  inputHandler(e) {
    this.setState({ [e.target.name]: e.target.value });
  }

  async loginRequestHandler() {
    try {
      await axios({
        method: "post",
        url: "https://localhost:4000/users/login",
        headers: {
          accept: "application/json",
        },
        data: {
          userId: this.state.username,
          password: this.state.password,
        },
        withCredentials: true,
      });
      this.props.loginHandler();
      const result = await axios({
        method: "get",
        url: "https://localhost:4000/users/userinfo",
        headers: {
          accept: "application/json",
        },
        withCredentials: true,
      });
      const { userId, email } = result.data.data;
      this.props.setUserInfo({
        userId,
        email,
      });
    } catch (err) {
      alert(err);
    }
  }
```

1. 서버측에서는 다음과 같은 작업이 진행됩니다.
    
    userId와 password를 담은 로그인 요청을 받으면 database에서 해당하는 유저가 있는지 조회합니다. 해당하는 유저가 없다면 request의 status는 400이 됩니다. index.js에서 session을 설정했으므로 req는 session 객체를 지니게 됩니다. session 객체에 현재 로그인한 유저의 정보는 저장합니다. 마지막으로, index.js에서 session을 설정햇으므로 response에 자동적으로 session이 담기게 됩니다()아마?. session은 로그인 상태를 유지하는데 쓰입니다.
    

```jsx
// server/controller/users/login.js

// 해당 모델의 인스턴스를 models/index.js에서 가져옵니다.
const { Users } = require("../../models");

module.exports = {
  post: async (req, res) => {
    // userInfo는 유저정보가 데이터베이스에 존재하고, 완벽히 일치하는 경우에만 데이터가 존재합니다.
    // 만약 userInfo가 NULL 혹은 빈 객체라면 전달받은 유저정보가 데이터베이스에 존재하는지 확인해 보세요

    try {
      const userInfo = await Users.findOne({
        where: { userId: req.body.userId, password: req.body.password },
      });

      // TODO: userInfo 결과 존재 여부에 따라 응답을 구현하세요.
      // 결과가 존재하는 경우 세션 객체에 userId가 저장되어야 합니다.
      if (userInfo === null) {
        return res.status(400).send({ data: null, message: "not authorized" });
      }
      // your code here
      // HINT: req.session을 사용하세요.
      delete userInfo.dataValues.password;
      req.session.user = userInfo.dataValues;

      return res.status(200).send({ data: userInfo, message: "ok" });
    } catch (err) {
      return res.status(500).send({ data: null, message: "server error" });
    }
  },
};
```

1. logout 요청이 들어오면 session객체의 destroy 메서드를 사용하여 session을 파기합니다.

```jsx
// server/controller/users/logout.js

module.exports = {
  post: (req, res) => {
    // TODO: 세션 아이디를 통해 고유한 세션 객체에 접근할 수 있습니다.
    // 앞서 로그인시 세션 객체에 저장했던 값이 존재할 경우, 이미 로그인한 상태로 판단할 수 있습니다.
    // 세션 객체에 담긴 값의 존재 여부에 따라 응답을 구현하세요.
    if (!req.session.userId) {
      // your code here
      res.status(400).json();
    } else {
      // your code here
      // TODO: 로그아웃 요청은 세션을 삭제하는 과정을 포함해야 합니다.
      req.session.destroy();
      res.status(200).json();
    }
  },
};
```

# Token-flow

1. client는 token을 state의 형태로 보관합니다. loginHandler로 token을 받아 issueAccessToken 메서드로 token을 state에 저장합니다. Mypage component는 token과 token을 state에 저장하기 위한 issueAccessToken 메서드를 props로 받습니다.

```jsx
// client/src/app.js

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isLogin: false,
      accessToken: "",
    };

    this.loginHandler = this.loginHandler.bind(this);
    this.issueAccessToken = this.issueAccessToken.bind(this);
  }

  loginHandler(data) {
    this.setState({ isLogin: true });
    this.issueAccessToken(data.data.accessToken);
  }

  issueAccessToken(token) {
    this.setState({ accessToken: token });
  }

  render() {
    const { isLogin } = this.state;
    return (
      <div className="App">
        {isLogin ? (
          <Mypage
            accessToken={this.state.accessToken}
            issueAccessToken={this.issueAccessToken}
          />
        ) : (
          <Login loginHandler={this.loginHandler} />
        )}
      </div>
    );
  }
}
```

1. client가 userId와 pssword를 담아 login request를 보냅니다. login에 성공하면 server는 token을 전달합니다.

```jsx
// client/src/components.login.js

class Login extends Component {
  constructor(props) {
    super(props);
    this.state = {
      userId: "",
      password: "",
    };
    this.inputHandler = this.inputHandler.bind(this);
    this.loginRequestHandler = this.loginRequestHandler.bind(this);
  }

  inputHandler(e) {
    this.setState({ [e.target.name]: e.target.value });
  }

  async loginRequestHandler() {
    /*
    TODO: Login 컴포넌트가 가지고 있는 state를 이용해 로그인을 구현합니다.
    로그인을 담당하는 api endpoint에 요청을 보내고, 받은 데이터로 상위 컴포넌트 App의 state를 변경하세요.
    초기 App:
    state = { isLogin: false, accessToken: "" }
    로그인 요청 후 App:
    state = { isLogin: true, accessToken: 서버에_요청하여_받은_access_token }
    */
    const { userId, password } = this.state;
    try {
      const result = await axios({
        method: "post",
        url: "https://localhost:4000/login",
        headers: {
          accept: "application/json",
        },
        withCredentials: true,
        data: { userId, password },
      });

      this.props.loginHandler(result.data);
    } catch (err) {
      alert(err);
    }
  }
```

1. login 요청이 들어오면 database에서 user의 정보를 조회합니다. 해당하는 유저가 존재한다면, password를 제외한 나머지 정보를 담은 토큰을 생성합니다. 최초의 로그인이므로 access token과 refresh token을 함께 생성합니다. access token은 response body로, refresh token은 cookie로 전달됩니다.
    
    
    token의 생성에는 jwt(json web token) library가 사용됩니다.
    

```jsx
// server/controllers/users/login.js

const { Users } = require("../../models");
const jwt = require("jsonwebtoken");
const dotenv = require("dotenv");

module.exports = async (req, res) => {
  // TODO: urclass의 가이드를 참고하여 POST /login 구현에 필요한 로직을 작성하세요.
  const { userId, password } = req.body;

  const userInfo = await Users.findOne({
    where: { userId: userId, password: password },
  });
  if (!userInfo) {
    res.status(400).json({ data: "null", message: "not authorized" });
  } else {
    delete userInfo.dataValues.password;

    const accessToken = jwt.sign(
      {
        ...userInfo.dataValues,
      },
      process.env.ACCESS_SECRET,
      { expiresIn: "1h" }
    );
    const refreshToken = jwt.sign(
      {
        ...userInfo.dataValues,
      },
      process.env.REFRESH_SECRET,
      { expiresIn: "1d" }
    );

    res
      .cookie("refreshToken", refreshToken)
      .status(200)
      .json({
        data: { accessToken: accessToken },
        message: "ok",
      });
  }
};
```

1. client가 authorization header에 token을 담아 요청을 보내면 server는 client에게 user의 정보를 전달합니다. client는 전달 받은 user의 정보를 state에 담아 rendering에 사용합니다.

```jsx
// client/src/component/mypage.js

async accessTokenRequest() {
    /* 
    TODO: 상위 컴포넌트인 App에서 받은 props를 이용해 accessTokenRequest 메소드를 구현합니다.
    access token을 처리할 수 있는 api endpoint에 요청을 보내고, 받은 데이터로 Mypage 컴포넌트의 state (userId, email, createdAt)를 변경하세요
    초기 Mypage:
    state = { userId: "", email: "", createdAt: "" }
    accessTokenRequest 후 Mypage:
    state = { userId: "특정유저id", email: "특정유저email", created: "특정유저createdAt" }
    
    ** 주의사항 **
    App 컴포넌트에서 내려받은 accessToken props를 authorization header에 담아 요청을 보내야 합니다. 
    */
    try {
      const result = await axios({
        method: "get",
        url: "https://localhost:4000/accesstokenrequest",
        headers: {
          accept: "application/json",
          authorization: `Bearer ${this.props.accessToken}`,
        },
        withCredentials: true,
      });
      const { createdAt, userId, email } = result.data.data.userInfo;
      this.setState({ createdAt, userId, email });
    } catch (err) {
      alert(err);
    }
  }
```

1. client가 header애 token을 담아 보내면 server는 해당 토큰이 유효한지, jwt의 verify를 사용하여 검증합니다. 토큰이 유효하면 db에서 해당 유저를 찾아 유저의 정보를 client에게 보냅니다.

```jsx
// server/controllers/accesstoken.js

module.exports = async (req, res) => {
  // TODO: urclass의 가이드를 참고하여 GET /accesstokenrequest 구현에 필요한 로직을 작성하세요.
  if (!req.headers.authorization) {
    res.status(404).json({ data: null, message: "invalid access token" });
  }

  const token = req.headers["authorization"].split(" ")[1];
  const data = jwt.verify(token, process.env.ACCESS_SECRET);

  const userInfo = await Users.findOne({
    where: { userId: data.userId },
  });
  delete userInfo.dataValues.password;

  if (!data) {
    res
      .status(400)
      .json({ data: null, message: "access token has been tempered" });
  } else {
    res.status(200).json({
      data: { userInfo: { ...userInfo.dataValues } },
      message: "ok",
    });
  }
};
```

1. client가 refresh token을 사용하여 server에 access token 발급 요청을 보냅니다. 새로운 access token을 받으면 props로 받은 issueAccessToken으로 token을 전달, state에 새로운 토큰을 저장합니다.

```jsx
// client/src/components/mypage.js

async refreshTokenRequest() {
    /*
    TODO: 쿠키에 담겨져 있는 refreshToken을 이용하여 refreshTokenRequest 메소드를 구현합니다.
    refresh token을 처리할 수 있는 api endpoint에 요청을 보내고, 받은 데이터로 2가지를 구현합니다.
    1. Mypage 컴포넌트의 state(userId, email, createdAt)를 변경
    2. 상위 컴포넌트 App의 state에 accessToken을 받은 새 토큰으로 교환
    */
    try {
      const result = await axios({
        method: "get",
        url: "https://localhost:4000/refreshtokenrequest",
        headers: {
          accept: "application/json",
        },
        withCredentials: true,
      });
      const { createdAt, userId, email } = result.data.data.userInfo;
      this.setState({ createdAt, userId, email });
      this.props.issueAccessToken(result.data.data.accessToken);
    } catch (err) {
      alert(err);
    }
  }
```

1. server는 refresh token과 함께 새로운 access token 발급 요청을 받으면 위와 마찬가지로 jwt의 verify로 refresh token이 유효한지 검증합니다. refresh token이 유효하다면 db에서 해당하는 유저를 조회, 유저가 존재한다면 새로운 access token을 만들고 유저의 정보와 함께 client에게 전달합니다.

```jsx
// server/controllers/refreshTokenRequest.js

module.exports = async (req, res) => {
  // TODO: urclass의 가이드를 참고하여 GET /refreshtokenrequest 구현에 필요한 로직을 작성하세요.
  const refreshToken = req.cookies.refreshToken;

  if (!refreshToken) {
    res.status(400).json({ data: null, message: "refresh token not provided" });
  }

  if (refreshToken === "invalidtoken") {
    res.status(400).json({
      data: null,
      message: "invalid refresh token, please log in again",
    });
  }

  const data = jwt.verify(refreshToken, process.env.REFRESH_SECRET);

  const userInfo = await Users.findOne({
    where: { userId: data.userId },
  });
  delete userInfo.dataValues.password;

  if (!userInfo) {
    res
      .status(400)
      .json({ data: null, message: "refresh token has been tempered" });
  } else {
    const accessToken = jwt.sign(
      {
        ...userInfo.dataValues,
      },
      process.env.ACCESS_SECRET,
      { expiresIn: "1h" }
    );

    res.status(200).json({
      data: { accessToken: accessToken, userInfo: userInfo },
      message: "ok",
    });
  }
};
```

# 220527

# Express.js로 서버 만들기

디렉토리 구성

![스크린샷 2022-05-27 22.03.36.png](220527%2064df1d260858416daea6e79e4d8e25c3/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-27_22.03.36.png)

app.js

```jsx
const port = 3001;

const flightRouter = require("./router/flightRouter");
const bookRouter = require("./router/bookRouter");
const airportRouter = require("./router/airportRouter");

app.use(cors());
app.use(express.json());

app.use("/flight", flightRouter);
app.use("/book", bookRouter);
app.use("/airport", airportRouter);

app.get("/", (req, res) => {
  res.status(200).send("Welcome, States Airline!");
});

app.use((req, res, next) => {
  res.status(404).send("Not Found!");
});

app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send({
    message: "Internal Server Error",
    stacktrace: err.toString(),
  });
});

app.listen(port, () => {
  console.log(`[RUN] StatesAirline Server... | http://localhost:${port}`);
});

module.exports = app;
```

routers

```jsx
//flightRouter.js

const { findAll, findById, update } = require('../controller/flightController');
const express = require('express');
const router = express.Router();

// use query parameters
router.get('/', findAll);

//use path parameters
router.get('/:id', findById); 

router.put('/:id', update);

module.exports = router;

//대동소이하므로 나머지 라우터 생략
```

flightController.js

```jsx
const flights = require("../repository/flightList");

module.exports = {
  // [GET] /flight
  // 요청 된 departure_times, arrival_times, destination, departure 값과 동일한 값을 가진 항공편 데이터를 조회합니다.
  findAll: (req, res) => {
    if (Object.keys(req.query).length !== 0) {
/*
      const { destination, departure, departure_times, arrival_times } =
        req.query;
      let list = [];
      if (destination !== undefined) {
        flights.map((flight) => {
          if (flight.destination === destination) {
            list.push(flight);
          }
        });
      }
      if (departure !== undefined) {
        flights.map((flight) => {
          if (flight.departure === departure) {
            list.push(flight);
          }
        });
      }
      if (departure_times !== undefined) {
        flights.map((flight) => {
          if (flight.departure_times === departure_times) {
            list.push(flight);
          }
        });
      }
      if (arrival_times !== undefined) {
        flights.map((flight) => {
          if (flight.arrival_times === arrival_times) {
            list.push(flight);
          }
        });
      }
      const temp = list.filter((v, i) => list.indexOf(v) !== i);
      const set = new Set(temp);
      const result = [...set];
      return res.status(200).json(result);
*/
    }
    return res.status(200).json(flights);
  },
  // [GET] /flight/:id
  // 요청 된 id 값과 동일한 uuid 값을 가진 항공편 데이터를 조회합니다.
  findById: (req, res) => {
    if (req.params !== undefined) {
      const list = flights.filter((item) => item.uuid === req.params.id);

      return res.status(200).json(list);
    }
    return res.status(404).end();
  },

  // [PUT] /flight/:id 요청을 수행합니다.
  // 요청 된 id 값과 동일한 uuid 값을 가진 항공편 데이터를 요쳥 된 Body 데이터로 수정합니다.
  update: (req, res) => {
    let data = req.body;
    data.uuid = req.params.id;

    return res.status(200).json(data);
  },
};
```

/flight의 get요청 시간나면 수정해야지. 모든 검색의 경우의 수에 대비할려다보니 코드가 장황하고 구질구질해져 버렸다. 마음에 안든다.

bookController.js

```jsx
const flights = require("../repository/flightList");
// 항공편 예약 데이터를 저장합니다.
let booking = [];

module.exports = {
  // [GET] /book 요청을 수행합니다.
  // 전체 데이터 혹은 요청 된 flight_uuid, phone 값과 동일한 예약 데이터를 조회합니다.
  findById: (req, res) => {
    const { flight_uuid, phone } = req.query;
    if (flight_uuid) {
      const list = booking.filter((book) => book.uuid === flight_uuid);
      return res.states(200).json(list);
    }
    if (phone) {
      const list = booking.filter((book) => book.phone === phone);
      return res.states(200).json(...list);
    }

    return res.status(200).json(booking);
  },

  // [POST] /book 요청을 수행합니다.
  // 요청 된 예약 데이터를 저장합니다.
  // 응답으로는 book_id를 리턴합니다.
  // Location Header로 예약 아이디를 함께 보내준다면 RESTful한 응답에 더욱 적합합니다.
  // 참고 링크: https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#useful-post-responses
  create: (req, res) => {
    const { flight_uuid, name, phone } = req.body;
    if (!flight_uuid || !name || !phone) {
      return res.status(400).json({ message: "Invalid body" });
    }
    booking.push({ flight_uuid, name, phone });
    return res.status(201).json({ message: "Create success" });
  },

  // [DELETE] /book?phone={phone} 요청을 수행합니다.
  // 요청 된 phone 값과 동일한 예약 데이터를 삭제합니다.
  deleteById: (req, res) => {
    const { phone } = req.query;
    booking = booking.filter((item) => item.phone !== phone);

    return res.status(200).json(booking);
  },
};
```

airportController.js

```jsx
const airports = require('../repository/airportList');

module.exports = {
  // [GET] /airport?query={query} 요청을 수행합니다.
  // 공항 이름 자동완성 기능을 수행합니다!
  findAll: (req, res) => {
    if (req.query.query !== undefined) {
      console.log(req.query.query);
      const list = airports.filter((item) => {
        return item.code.includes(req.query.query.toUpperCase());
      });
      return res.status(200).json(list);
    }
    res.json(airports);
  }
};
```

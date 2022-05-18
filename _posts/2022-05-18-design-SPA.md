이거…는 나중에 하고

굳이 할 필요 있겄나 싶기도 하고

- resolve 와 reject는 무엇을 의미하남?
- resolve, reject 함수에 인자 넘길 수 있음. 이때 넘기는 인자는 어떻게 사용?
- new Promise를 통해 생성한 Promise 인스턴스에는 어떤 메소드가 존재? 용도는?
- Promise.prototype.then 메소드는 무엇을 리턴?
- Promise.prototype.catch 메소드는 무엇을 리턴?
- Promise의 세가지 상태?
- await 키워드 다음에 등장하는 함수 실행은 어떤 타입을 리턴할 경우에만 의미가 있나?
- await 키워드를 사용할 경우 어떤 값이 리턴?

내일 과제로 하게 될 gallery 설계나 해보자

```jsx
const photo = [{id:00,path:''}...]

const 썸네일 = (photo) => {

const {curPhoto, setCurphoto} = useState(photo[01])
const photoHandle = (e) => {
	setCurphoto(e.target)

const PhotoGen = () => 
return <div>
				{photo.map((el.id, el.path )=> return <img id path onClick(photohandle)/> )}
				</div>

const Gallery = () => {
return <div>
				<PhotoGen/>
				<img src='cur'/>
			</div>

```

대충 이런 느낌적 느낌? 내 예상이 맞다면 스테이트 많이 사용할 필요 없을 것 같다.

app.js는

```jsx
const App = (props) => {
  return (
    <BrowserRouter>
      <div className="App">
        <main>
          <nav/> //네비게이션 바. nav.js에서 링크 위치 달아주기
          <section className="features">
            {/* <Route exact path="/"> */}
            <Switch>
              <Route exact path="/">
                <Top />
              </Route>
              <Route path="/about">
                <About />
              </Route>
              <Route path="/mypage">
                <Gallery />
              </Route>
            </Switch>

            {/* </Route> */}
            {/* TODO : 유어클래스를 참고해서, 테스트 케이스를 통과하세요.
            TODO : React Router DOM 설치 후 BrowserRouter, Route의 주석을 해제하고 Swtich 컴포넌트를 적절하게 작성합니다. */}
          </section>
        </main>
      </div>
    </BrowserRouter>
  );
};
```

상단 네비는 nav.js

```jsx
const Nav = () => {
  return (
    <section className="navbar">
      {/* TODO : Link 컴포넌트를 작성하고, to 속성을 이용하여 경로(path)를 연결합니다. */}
      <Link to="/">
        <i className="far fa-comment-dots"></i>
      </Link>
      <Link to="/about">
        <i className="far fa-question-circle"></i>
      </Link>
      <Link to="/gallery">
        <i className="far fa-user"></i>
      </Link>
    </section>
  );
};
```

아이고 의미 없다.  내일 직접 해봐야지… 의미 있을…라나.있다고 치자.

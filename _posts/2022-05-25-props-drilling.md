# 220524

# How to avoid props drilling?

먼저 예제를 보자

## App.js

![Untitled](220524%20552d039998a349b48ce38f23165c622d/Untitled.png)

## ItemListContainer.js

![스크린샷 2022-05-25 20.34.54.png](220524%20552d039998a349b48ce38f23165c622d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-25_20.34.54.png)

## Item.js

![스크린샷 2022-05-25 20.35.58.png](220524%20552d039998a349b48ce38f23165c622d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-25_20.35.58.png)

App의 handleAddCartItems는 Items에서 요구되지만, Items에서 바로 쓸 수는 없다. 따라서 ItemListContainer에서 필요로 하지 않음에도 불과하고 , 우선 ItemListContiner를 거치야 한다.

 

![스크린샷 2022-05-25 20.51.28.png](220524%20552d039998a349b48ce38f23165c622d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-25_20.51.28.png)

App-ShoppingCart-CartItem 순으로 handleDeleteItem이 전달되며 같은 문제가 발생하고 있다.

예제처럼 2,3개 정도의 컴포넌트 사이에서 drilling이 발생하는 경우는 양호한 편이라 할 수있겠다. 더 많은 컴포넌트 사이에서 같은 일이 벌어진다면 유지보수의 난이도가 수직상승할 것이다.

그렇다면 해결 방법은 무엇인가?

- 상태관리 라이브러리(Redux 등)을 이용하여 전역 상태 저장소 제공
- Context Api

마지막 옵션은 아래 링크에서.

[https://blog.logrocket.com/solving-prop-drilling-react-apps/](https://blog.logrocket.com/solving-prop-drilling-react-apps/)

결론은 component composition을 이용하는 것인데…

다음 시간에는 Composition과 Inheritance를 비교해보겠다. 끝

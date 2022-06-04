## Redux란?
Redux는 자바스크립트 상태관리 라이브러리이다.

## 상태관리도구의 필요성
### 상태란?
React에서 State는 component 안에서 관리되는 것이다.   
아래는 Class component 상태 관리 예시이다.   
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state  = {date: new Date()}; // 여기서 관리
  }

  render() {
    return (
        <div>
          <h1>Hello World!</h1>
          <h2>It's {this.state.data.toLocaleTimeString()}.</h2>
        </div>
    );
  }
}
```
<br/>

### Component 간의 정보 공유
자식 컴포넌트들 간의 다이렉트 데이터 전달은 불가능 하다.   
자식 컴포넌트들 간의 데이터를 주고 받을 때는 상태를 관리하는 부모 컴포넌트를 통해서 주고 받는다.   
그런데 자식이 많아진다면 상태 관리가 매우 복잡해진다.   
상태를 관리하는 상위 컴포넌트에서 계속 내려 받아야한다. => Props drilling 이슈   
How to solve a Problem with React?   

### 상태 관리 툴은 어떤 문제를 해결해 주나?
1. 전역 상태 저장소 제공   
2. Props drilling 이슈 해결   
예를 들어, <A>라는 컴포넌트에 상태가 있고, <I>라는 컴포넌트가 해당 상태를 사용한다고 하면,   
그 중간에 존재하는 <C>, <G> 등은 굳이 name이라는 상태가 필요하지 않음에도,   
컴포넌트에 props를 만들어 자식 컴포넌트에 넘겨주어야 했다.   
이를 props drilling(프로퍼티 내려꽂기) 문제라고 부른다.   
전역 상태 저장소가 있고, 어디서든 해당 저장소에 접근할 수 있다면 이러한 문제는 해결 된다.   
<br/>

## Redux의 기본개념
1. Single source of truth
* 동일한 데이터는 항상 같은 곳에서 가지고 온다.
* 즉, 스토어라는 하나뿐인 데이터 공간이 있다는 의미이다.
2. State is read-only
* 리액트에서는 setState 메소드를 활용해야만 상태 변경이 가능하다.
* 리덕스에서도 액션이라는 객체를 통해서만 상태를 변경할 수 있다.
3. Changes are made with pure functions
* 변경은 순수함수로만 가능하다.
* 리듀서와 연관되는 개념이다.
* Store(스토어) – Action(액션) – Reducer(리듀서)

## Store, Action, Reducer의 의미와 특징

### Store (스토어)
* Store(스토어)는 상태가 관리되는 오직 하나의 공간이다.
* 컴포넌트와는 별개로 스토어라는 공간이 있어서 그 스토어 안에 앱에서 필요한 상태를 담는다.
* 컴포넌트에서 상태 정보가 필요할 때 스토어에 접근한다.
### Action (액션)
* Action(액션)은 앱에서 스토어에 운반할 데이터를 말한다. (주문서)
* Action(액션)은 자바스크립트 객체 형식으로 되어있다.
### Reducer (리듀서)
* Action(액션)을 Store(스토어)에 바로 전달하는 것이 아니다.
* Action(액션)을 Reducer(리듀서)에 전달해야한다.
* Reducer(리듀서)가 주문을 보고 Store(스토어)의 상태를 업데이트하는 것이다.
* Action(액션)을 Reducer(리듀서)에 전달하기 위해서는 dispatch() 메소드를 사용해야한다.

## Redux의 장점
* 상태를 예측 가능하게 만든다. (순수함수를 사용하기 때문)
* 유지보수 (복잡한 상태 관리와 비교)
* 디버깅에 유리 (action과 state log 기록 시) → redux dev tool (크롬 확장)
* 테스트를 붙이기 용의 (순수함수를 사용하기 때문)
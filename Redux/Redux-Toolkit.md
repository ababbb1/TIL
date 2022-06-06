# Redux Toolkit
Redux 툴킷 패키지는 기존 Redux에 대한 아래 3가지 문제를 해결하기 위해 만들어졌습니다.
* "Redux 스토어 구성이 너무 복잡합니다"
* "Redux에서 유용한 작업을 수행하려면 많은 패키지를 추가해야 합니다."
* "Redux에는 너무 많은 보일러플레이트 코드(반복적인 기본 구성 코드)가 필요합니다."
   
Redux 툴킷에는 액션 관리를 위한 redux-actions, 불변성 보존을 위한 immer, store 값을 효율적으로 핸들링하여 불필요한 리렌더링을 막기위한 reselect, 비동기 작업을 위한 thunk가 기본적으로 내장되어있습니다.

## Redux 툴킷 사용
Redux 툴킷 사용을 위해서 두가지 패키지가 필요합니다.   
```bash
npm install @reduxjs/toolkit react-redux
```
<br/>

store를 생성하기 위한 파일을 추가합니다.   
아래는 빈 Redux 저장소를 만들고 export하는 기본 코드입니다.   
Redux 저장소가 생성되고 Redux Devtools 확장도 자동으로 구성됩니다.   
```javascript:store.js
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```
<br/>

index.js 파일에서 App 컴포넌트 태그를 Provider 태그로 감싸줍니다.   
```javascript:index.js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import { store } from './app/store'
import { Provider } from 'react-redux'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
<br/>

슬라이스를 생성하는 파일을 추가합니다.   
슬라이스를 생성하려면 슬라이스를 식별하기 위한 문자열 이름, 초기 상태 값, 상태 업데이트 방법을 정의하는 하나 이상의 리듀서 함수가 필요합니다. 슬라이스가 생성되면 생성된 Redux 액션 생성자와 전체 슬라이스에 대한 리듀서 기능을 내보낼 수 있습니다.
<br/>

Redux는 데이터 복사본을 만들고 복사본을 업데이트하여 모든 상태 업데이트를 불변적으로 작성 하도록 요구합니다.   
Redux 툴킷은 immer.js 라이브러리가 내장되어 있어 createSlice와 createReducer API를 통해 불변성을 제공합니다.   
아래는 카운터 기능을 위한 슬라이스 파일 생성 예시입니다.   
```javascript:counterSlice.js
import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  value: 0,
}

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1
    },
    decrement: (state) => {
      state.value -= 1
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload
    },
  },
})

export const { increment, decrement, incrementByAmount } = counterSlice.actions

export default counterSlice.reducer
```
<br/>

위의 store.js 파일에 counterSlice.js 파일에서 가져온 리듀서 함수를 추가합니다.   
매개변수에 들어가는 객체의 reducer 함수가 해당 상태에 대한 모든 업데이트를 처리합니다.   
```javascript
import { configureStore } from '@reduxjs/toolkit'
import counterReducer from '../features/counter/counterSlice'

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
})
```

React-Redux의 useSelector hook을 사용하여 데이터를 읽고   
useDispatch를 사용하여 작업을 전달할 수 있습니다.   
```javascript:Counter.js
import React from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { decrement, increment } from './counterSlice'

export function Counter() {
  const count = useSelector((state) => state.counter.value)
  const dispatch = useDispatch()

  return (
    <div>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  )
}
```
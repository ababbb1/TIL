# This
자신을 호출한 객체를 가리킨다.   
```javascript
const prop = 10;

const test = {
  prop: 20,
  func: function() {
    return this.prop;
  },
};

console.log(test.func()); // 20
```
   
아래의 f 함수는 전역 객체(window)의 메소드이기 때문에 this는 window이다.   
```javascript
function f() {
  return this;
}

f(); // window
```
   
고차 함수의 콜백 안에서의 this는 콜백이 일반 함수이기 때문에 전역 객체를 참조한다.   
```javascript
const test = {
  testArr = [1, 2, 3],
  printThis() {
    console.log(this); // test
    this.testArr.forEach(function() {
      console.log(this); // window
    });
  }
}
```
   
일반 함수는 자신이 어떻게 호출되는지에 따라 this에 바인딩할 객체가 동적으로 결정되지만,   
화살표 함수 안에서의 this는 언제나 상위 스코프의 this를 가리킨다.   
```javascript
const test = {
  testArr = [1, 2, 3],
  printThis() {
    console.log(this); // test
    this.testArr.forEach(() => {
      console.log(this); // test
    });
  }
}
```
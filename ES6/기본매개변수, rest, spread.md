## 기본 매개 변수
ES6에서는 매개변수 기본값을 사용할 수 있다. 매개변수 기본값은 매개변수에 인수를 전달하지 않았을 경우에만 유효하다.   
```javascript
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1));    // 1
console.log(sum(1, 2)); // 3
```
<br/>

## Rest 파라미터
Rest 파라미터는 매개변수 이름 앞에 세개의 점 ...을 붙여서 정의한 매개변수를 의미한다. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.   
Rest 파라미터는 반드시 마지막 파라미터여야 한다.   
```javascript
function f1(head, ...rest) {
  console.log(head);
  console.log(rest);
}

f1(1, 2, 3, 4, 5); // 1 [2, 3, 4, 5]

function f2(a, b, ...rest) {
  console.log(a);
  console.log(b);
  console.log(rest);
}

f2(1, 2, 3, 4, 5); // 1 2 [3, 4, 5]
```
<br/>

## Spread 문법
Spread 문법( ... )는 대상을 개별 요소로 분리한다. Spread 문법의 대상은 이터러블/이터레이터 프로토콜을 따라야 한다.   
```javascript
console.log(...[1, 2, 3]) // 1, 2, 3
console.log(...'Hello');  // H e l l o
console.log(...new Map([['a', '1'], ['b', '2']]));  // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3]));  // 1 2 3

// 객체는 이터러블/이터레이터 프로토콜을 따르지 않아 오류가 발생한다.
console.log(...{ a: 1, b: 2 }); // TypeError: obj is not iterable

// ES2018 이후 부터는 객체 내부에서 Spread 문법을 사용할 수 있다.
const obj1 = { name: 'John', age: '20' };
const obj2 = { name: 'Tom', age: '23' };

const clonedObj1 = { ...obj1 }; // { name: 'John', age: '20 }
const mergedObj = { ...obj1, ...obj2 }; // { name: 'John', age: '20', name: 'Tom', age: '23' }

// clonedObj1은 obj1의 프로퍼티만 복사해왔기 때문에 객체 자체의 참조값은 다르다.
console.log(obj1 === clonedObj1); // false



// 배열을 분해하여 배열의 각 요소를 파라미터에 전달할 수 있다.
// 위의 Rest 파라미터 문법과의 차이점에 유의해야한다.
function f1(a, b, c) {
  console.log(a);
  console.log(b);
  console.log(c);
}

function f2(a, ...rest) {
  console.log(a);
  console.log(rest);
}

const params = [1, 2, 3];
f1(...params); // 1 2 3
f2(params) // 1 [2, 3]



// concat
const arr = [1, 2, 3];
console.log([...arr, 4, 5, 6]); // [ 1, 2, 3, 4, 5, 6 ]



// push
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

arr1.push(...arr2);
console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]



// splice
const arr1 = [1, 2, 3, 6];
const arr2 = [4, 5];

arr1.splice(3, 0, ...arr2);
console.log(arr1); // [ 1, 2, 3, 4, 5, 6 ]
```


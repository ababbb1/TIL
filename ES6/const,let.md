## const, let

### 기존 var의 단점
* 변수 중복 선언이 가능하여, 예기치 못한 값을 반환할 수 있다.
* 함수 레벨 스코프로 인해 함수 외부에서 선언한 변수는 모두 전역변수가 된다.
* 변수 선언문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

### let
변수 중복 선언이 불가하지만, 재할당은 가능하다.

```javascript
let name = 'John';
console.log(name); //John

let name = 'Jane';
console.log(name); //Uncaught SyntaxError: Identifier 'name' has already been declared

name = 'Jane';
console.log(name); //Jane
```

### const
* 변수 중복 선언 불가
* 재할당 불가 (객체 내부 프로퍼티는 변경 가능)
* 선언과 초기화가 동시에 진행되어야 한다.

```javascript
const name; //Uncaught SyntaxError: Missing initializer in const declaration

const name = 'John';
name = 'Jane' //Uncaught TypeError: Assignment to constant variable.

const user = {
  name: 'John'
}
user.name = 'Jane';
console.log(user.name); //Jane
```

### 블록 레벨 스코프
<span style="background-color: #ffdce0;">let</span>, <span style="background-color: #ffdce0;">const</span>로 선언한 변수들은 코드블록(ex. function, if, for, while, try/catch)을   
지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```javascript
var a = 1;

if (true) {
  var a = 2;
}
console.log(a); //2

let b = 1;

if if (true) {
  let b = 2;
}
console.log(b); //1
```

### 변수 호이스팅
<span style="background-color: #ffdce0;">let</span>으로 선언한 변수는 **선언 단계와 초기화 단계가 분리**되어 진행된다.   
런타임 이전에 자바스크립트 엔진에 의해 선언 단계가 실행되지만,   
초기화 단계가 실행되지 않았을 때 해당 변수에 접근하려고 하면 참조 에러가 뜬다.   
따라서 let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점까지   
변수를 참조할 수 없는 **일시적 사각지대(Temporal Dead Zone: TDZ)** 구간에 존재한다.

```javascript
console.log(name) // output: Uncaught ReferenceError: name is not defined
let name = 'John';
```

<span style="background-color: #ffdce0;">const</span>는 **선언과 초기화가 동시에 진행**된다.

```javascript
console.log(name) // output: Uncaught ReferenceError: Cannot access 'name' before initialization
const name = 'John'
```
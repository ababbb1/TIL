# Prototype

자바스크립트는 기존의 객체를 복사하여 새로운 객체를 생성하는 프로토타입 기반의 언어이다.   
이렇게 복사된 객체는 다른 객체의 원형이 될 수 있다.   
프로토타입은 객체를 확장하고 클래스 없이도 객체 지향적인 프로그래밍을 할 수 있게 해준다.   
자바스크립트에서는 기본 데이터 타입인 boolean, number, string, null, undefined 빼고는 모두 객체이다.   
모든 객체는 \_\_proto\_\_를 멤버로 가지고 있고, 함수 객체는 prototype 객체를 멤버로 가지고 있다.   
```javascript
function User(){};

/* 아래는 실제 구현이 아닌 시각화를 위한 것입니다.

User {
  __proto__: Function.prototype,
  prototype: User.prototype
}

User.prototype {
  __proto__: Object.prototype,
  constructor: User
}

*/
```
   
User 함수의 prototype 속성이 참조하는 프로토타입 객체는   
new라는 연산자와 프로토타입 객체의 constructor인   
User 함수를 통해 생성된 모든 객체의 원형이 되는 객체이다.   
```javascript
function User(){};

const user1 = new User();
const user2 = new User();

/* 아래는 실제 구현이 아닌 시각화를 위한 것입니다.

User {
  __proto__: Function.prototype,
  prototype: User.prototype
}

User.prototype {
  __proto__: Object.prototype,
  constructor: User
}

user1 {
  __proto__: User.prototype
}

user2 {
  __proto__: User.prototype
}

*/
```
   
프로토타입 객체에 동적으로 런타임에 멤버를 추가할 수 있다.   
프로토타입 객체를 원형으로 생성된 모든 객체는   
프로토타입 객체의 멤버를 자신의 것처럼 사용할 수 있다.   
```javascript
function User(){};

User.prototype.sayHi = function() {
  console.log('Hi');
}

const user1 = new User();
const user2 = new User();

user1.sayHi(); // 'Hi'
user2.sayHi(); // 'Hi'

/* 아래는 실제 구현이 아닌 시각화를 위한 것입니다.

User {
  __proto__: Function.prototype,
  prototype: User.prototype
}

User.prototype {
  __proto__: Object.prototype,
  constructor: User,

  sayHi: function() {
    console.log('Hi');
  }
}

user1 {
  __proto__: User.prototype
}

user2 {
  __proto__: User.prototype
}

*/
```
   
프로토타입 객체의 생성자 함수로 생성된 객체는   
자신의 멤버를 수정해도 프로토타입 객체에는 영향을 주지 않는다.   
```javascript
function User(){};

User.prototype.sayHi = function() {
  console.log('Hi');
}

const user1 = new User();
const user2 = new User();

user1.sayHi = function () {
  console.log('Hi user1');
}

user2.name = 'Jane';

user1.sayHi(); // 'Hi user1'
user2.sayHi(); // 'Hi'
console.log(user1.name); // undefined
console.log(user2.name); // 'Jane'

/* 아래는 실제 구현이 아닌 시각화를 위한 것입니다.

User {
  __proto__: Function.prototype,
  prototype: User.prototype
}

User.prototype {
  __proto__: Object.prototype,
  constructor: User,

  sayHi: function() {
    console.log('Hi');
  }
}

user1 {
  __proto__: User.prototype,

  sayHi: function() {
      console.log('Hi user1');
    }
}

user2 {
  __proto__: User.prototype,

  name: 'Jane'
}

*/
```
   
## 상속
자바스크립트의 객체간 상속 구현 방법은 두가지 방법이 있다.   
### classical 방식
classical 방식은 new 연산자를 사용하여 Java에서 객체를 생성하는   
방식과 유사한 방식이다.   
```javascript
function Person(name = 'John') {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
}

function User(name) {
  this.name = name;
}
User.prototype = Person.prototype;

const user = new User('Jane');
console.log(user.name); // 'Jane'

/* 아래는 실제 구현이 아닌 시각화를 위한 것입니다.

Person {
  __proto__: Function.prototype,
  prototype: Person.prototype
}

Person.prototype {
  __proto__: Object.prototype,
  constructor: Person

  getName: function(name = 'John') {
    return this.name;
  }
}

User {
  __proto__: Function.prototype,
  prototype: User.prototype
}

User.prototype {
  __proto__: Object.prototype,
  constructor: Person

  getName: function(name = 'John') {
    return this.name;
  }
}

user {
  __proto__: User.prototype,

  name: 'Jane'
}

*/
```
   
### prototypal한 방식
```javascript
const person = {
  type: 'person',
  getType: function() {
    return this.type;
  },
  getName: function() {
    return this.name;
  }
}

const person1 = Object.create(person);
person1.name = 'John';

console.log(person1.getType()); // 'person'
console.log(person1.getName()); // 'John'
```
## 객체 리터럴

### 객체란?

- 자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 모든 것이 객체
- 원시 값을 제외한 나머지 값으로 변경 불가능한 원시 값과 달리 객체 값은 변경 가능
- 0개 이상의 프로퍼티로 구성된 집합이며, 키와 값으로 구성

---

### 객체 생성

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스

---

### 프로퍼티 접근

```javascript
var person = {
  // 프로퍼티 키 : name / 프로퍼티 값 : Lee
  name: "Lee",
  sayHello: function () {
    // 메서드
    console.log(`hello my name is ${this.name}`); // this는 person을 가리킴
  },
};

console.log(typeof person); // object
console.log(person); // {name: Lee, sayHello: f}

// 프로퍼티 키 동적 생성
var obj = {};
var key = "hello";

obj[key] = "world";

consol.log(obj); // {hello : world}

// 프로퍼티 키 중복 선언
var foo = {
  name: "Lee",
  name: "Kim",
};

console.log(foo); // {name : 'Kim'}

// 마침표 표기법에 의한 프로퍼티 접근
console.log(foo.name); // Kim

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(foo["name"]); // Kim

// 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는
// 반드시 따옴표로 감싼 문자열이어야 함
console.log(foo[name]); // ReferenceError : name is not defined

// 프로퍼티 삭제
delete foo.name;
```

---

### 프로퍼티 축약 표현

```javascript
let x = 1,
  y = 2;

const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

---

### 계산된 프로퍼티 이름

```javascript
const prefix = "prop";
let i = 0;

const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
};

console.log(obj); // {prop-1 : 1, prop-2 : 2, prop-3 : 3}
```

---

### 메서드 축약 표현

```javascript
const obj = {
  name: "Lee",
  sayHi() {
    console.log("Hi" + this.name);
  },
};
obj.sayHi(); // Hi Lee
```

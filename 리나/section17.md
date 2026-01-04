## 생성자 함수에 의한 객체 생성

### `Object` 생성자 함수

- `new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체 생성하여 반환
- 빈 객체를 생성한 이후 프로퍼티 또는 메서드 추가하여 객체 완성시킬 수 있음

```js
// 빈 객체 생성
const person = new Object();

// 프로퍼티 추가
person.name = "Lee";
person.sayHello = function () {
  console.log("Hi " + this.name);
};

console.log(person); // {name: 'Lee', sayHello: f}
person.sayHello(); // Hi Lee
```

---

### 생성자 함수

- **`new` 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수**
- 생성자 함수에 의해 생성된 객체를 `인스턴스`라 함
- `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등의 빌트인 생성자 함수 제공
- **일반 함수와 동일한 방법으로 생성자 함수를 정의하고 `new` 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작**
- 만약 `new` 연산자와 함께 생성자 함수를 호출하지 않을 경우 일반 함수로 동작

#### 객체 리터럴에 의한 객체 생성 방식의 문제점

- 단 하나의 객체만 생성하기 때문에 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적임

#### 생성자 함수에 의한 객체 생성 방식의 장점

- 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성 가능

```js
// 객체 리터럴을 사용하여 객체 생성
const Circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); // 10

const Circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  },
};

console.log(circle.getDiameter()); // 20

// => 두 객체는 동일한 프로퍼티 구조를 가졌지만, 각각 프로퍼티를 기술해줘야 함

// 생성자 함수를 사용하여 객체 생성
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5); // 반지름이 5인 Circle 객체 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

// => 동일한 프로퍼티 구조를 갖는 객체를 간편하게 생성 가능
```

#### 생성자 함수의 인스턴스 생성 과정

1. 인스턴스 생성과 `this` 바인딩

- 함수 코드가 실행되기 전, 암묵적으로 빈 객체 생성
- 빈 객체 => 생성자 함수가 생성한 인스턴스
- 인스턴스 => `this`에 바인딩
  - 함수 내부의 `this`가 곧 생성될 인스턴스를 가리키게 됨

2. 인스턴스 초기화;

- 바인딩되어 있는 인스턴스에 프로퍼티나 메서드 추가
  - 이 과정에서 사용자로부터 받은 인수를 할당하여 인스턴스의 초기 상태 설정

3. 인스턴스 반환

- 함수 내부의 모든 처리가 끝나면, 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환

#### 만약 return 문이 명시적으로 있다면?

- 함수 내부에서 객체를 명시적으로 반환하면, `this` 대신 해당 객체가 반환됨
- but, 원시 값을 반환할 경우 무시하고 `this` 반환 <br />

=> 생성자 함수 내부에서는 명시적으로 `this`가 아닌 값을 반환하는 것은 생성자 함수의 기본 동작을 훼손하는 것이기 때문에 `return` 문을 반드시 생략하는 것이 관례임

```js
// 생성자 함수의 인스턴스 생성 과정
function Circle(radius) {
  // 1. 암묵적으로 빈 객체 생성, this에 바인딩

  // 2. this에 바인딩되어 있는 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 완성된 인스턴스가 바인딩된 this 암묵적으로 반환
}

// 인스턴스 생성
const circle = new Circle(1);
console.log(circle); // Circle { radius : 1, getDiameter : f}
```

```js
// this가 아닌 다른 객체 명시적 반환
function Circle(radius) {
  // 1. 암묵적으로 빈 객체 생성, this에 바인딩

  // 2. this에 바인딩되어 있는 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 명시적 객체 반환 => this 무시
  return {};
}

// 인스턴스 생성
const circle = new Circle(1);
console.log(circle); // {}
```

```js
// this가 아닌 원시 값 명시적 반환
function Circle(radius) {
  // 1. 암묵적으로 빈 객체 생성, this에 바인딩

  // 2. this에 바인딩되어 있는 인스턴스 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
  // 3. 명시적 객체 반환 => this 무시
  return 100;
}

// 인스턴스 생성
const circle = new Circle(1);
console.log(circle); // Circle { radius : 1, getDiameter : f}
```

---

### 내부 메서드 `[[Call]]과 [[Construct]]`

- 함수는 객체이지만 일반 객체와는 다름
- 일반 객체는 호출할 수 없지만 함수는 호출 가능
- 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 `[[Call]], [[Construct]]` 같은 내부 메서드를 추가로 갖고 있음
- 함수가 일반 함수로 호출되면 함수 객체의 내부 메서드 `[[Call]]` 호출 => `callable`
- `new` 연산자와 함께 생성자 함수로 호출되면 내부 메서드 `[[Construct]]` 호출 => `constructor`
- `[[Construct]]`를 갖지 않는 함수 객체 => `non-constructor` <br />

=> 함수 객체는 `callable`이면서 `constructor`이거나 `callable`이면서 `non-constructor`로 모든 함수 객체는 호출할 수 있지만 모든 함수 객체를 생성자 함수로서 호출할 수 있는 것은 아님

- `constructor` : 함수 선언문, 함수 표현식, 클래스
- `non-constructor` : 메서드(ES6 메서드 축약 표현), 화살표 함수

---

### `new.target`

- 생성자 함수가 `new` 연산자 없이 호출되는 것을 방지하기 위해 파스칼 케이스 컨벤션을 사용한다 하더라도 실수는 언제나 발생할 수 있음 <br />
  => 이러한 위험성을 회피하기 위해 ES6에서는 `new.target` 지원
- `new.target`은 `this`와 유사하게 `constructor`인 모든 함수 내부에서 암묵적인 지역 변수와 같이 사용되며 메타 프로퍼티라고 부름
- `new` 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 `new.target`은 함수 자신을 가리킴
- `new` 연산자 없이 일반 함수로서 호출된 함수 내부의 `new.target`은 `undefined` <br />
- `new.target`은 IE에서 지원하지 않기 때문에 이런 상황이라면, `스코프 세이프 생성자 패턴` 사용

=> 따라서 함수 내부에서 `new.target`을 사용하여 `new` 연산자와 생성자 함수로서 호출했는지 확인하여 그렇지 않은 경우 `new` 연산자와 함께 재귀호출을 통해 생성자 함수로서 호출 가능

```js
// new.target
function Circle(radius) {
  if (!new.target) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

```js
// 스코프 세이프 생성자 패턴
function Circle(radius) {
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle = new Circle(5);
console.log(circle.getDiameter()); //10
```

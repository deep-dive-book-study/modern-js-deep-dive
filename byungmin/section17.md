## 17장 생성자 함수에 의한 객체 생성

### 생성자 함수
1. 객체 리터럴 방식의 단점
- 재사용성 부족: 동일한 객체를 생성하려면 매번 똑같은 프로퍼티와 메서드를 직접 타이핑해야 한다.
- 유지보수 저하: 객체의 구조가 변경되면 수십 개의 리터럴 코드를 일일이 찾아 수정해야 한다.
- 메모리 낭비: 로직이 동일한 메서드(함수)를 객체마다 중복해서 생성하게 되어 메모리 효율이 떨어진다.
```javascript
// 객체 리터럴의 비효율성 예시
const circle1 = {
  radius: 5,
  getDiameter() { return 2 * this.radius; }
};

const circle2 = {
  radius: 10,
  getDiameter() { return 2 * this.radius; } // 로직 중복
};
```

2. 생성자 함수에 의한 객체 생성 방식
- 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출**
```javascript
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function () {
        return 2 * this.radius;
    };
}

// 인스턴스 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);
```

3. 생성자 함수의 인스턴스 생성 과정
<br> new 연산자와 함께 생성자 함수를 호출하면 다음과 같은 과정을 거친다.
- `인스턴스 생성과 this 바인딩`: 암묵적으로 빈 객체가 생성되고, 이 인스턴스 객체가 this에 바인딩된다.
- `인스턴스 초기화`: this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 초기화한다.
- `인스턴스 반환`: 모든 작업이 끝나면 완성된 객체(this)가 암묵적으로 반환된다.

### 내부 메서드 `[[Call]]`과 `[[Construct]]`
1. Callable (호출 가능한 객체)
- 의미: 내부 메서드 `[[Call]]`을 가진 함수 객체
- 범위: 모든 함수 객체는 Callable이다. 즉, 어떤 방식(선언문, 표현식, 화살표 함수 등)으로 정의하든 호출할 수 있으므로 반드시 `[[Call]]`을 가지고 있다.

2. Constructor (생성자 함수로 동작 가능한 객체)
- 의미: 내부 메서드 `[[Construct]]`를 가진 함수 객체
- 범위: 모든 함수가 `[[Construct]]`를 가지는 것은 아니다.
  - Constructor: 함수 선언문, 함수 표현식, 클래스
  - Non-constructor: 화살표 함수, 메서드 축약 표현`(obj = { foo() {} })`

### `constructor`와 `non-constructor`
자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 구분한다.

1. `Constructor` (생성자 함수로 호출 가능)
- `Constructor는` 내부 메서드 [[Construct]]를 가지고 있는 함수 객체, new 연산자와 함께 호출하여 객체(인스턴스)를 생성할 수 있다.
- 종류: 함수 선언문, 함수 표현식, 클래스(Class)
- 특징: 객체를 생성해야 하므로 `prototype` 프로퍼티를 가지고 있으며, `this` 바인딩 과정이 포함된다.
```javascript
// 1. 함수 선언문
function Person(name) {
  this.name = name;
}

// 2. 함수 표현식
const Car = function(model) {
  this.model = model;
};

// 둘 다 new 연산자와 함께 호출 가능
console.log(new Person('Kim')); // Person {name: "Kim"}
console.log(new Car('Tesla')); // Car {model: "Tesla"}
```

2. `Non-constructor` (생성자 함수로 호출 불가능)
- `Non-constructor`는 내부 메서드 [[Construct]]가 없는 함수 객체로, 호출은 가능하지만([[Call]] 보유), 객체를 생성하는 용도로는 사용할 수 없다.
- 종류: 화살표 함수, 메서드 축약 표현
- 특징: 생성자로서의 기능을 제거하여 함수가 가볍다. prototype 프로퍼티가 없으며, new와 함께 호출 시 에러가 발생한다.
```javascript
// 1. 화살표 함수
const add = (a, b) => a + b;

// 2. 메서드 축약 표현
const obj = {
  sayHi() {
    console.log('Hi!');
  }
};

// new 연산자와 함께 호출 시 TypeError 발생
new add(); // TypeError: add is not a constructor
new obj.sayHi(); // TypeError: obj.sayHi is not a constructor
```
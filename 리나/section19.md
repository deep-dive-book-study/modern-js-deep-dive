## 프로토타입

### 객체지향 프로그래밍

- 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
- 객체지향 프로그래밍은 실세계의 실체(사물이나 개념)를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작
- 실제는 특징이나 성질을 나타내는 속성을 가지고 있고, 이를 통해 실체를 인식하거나 구별 가능
- 추상화는 다양한 속성 중 프로그램에 필요한 속성만 간추려 내어 표현하는 것 <br />

=> **속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라 하며, 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 객체지향 프로그래밍이라 함** <br />
=> **객체지향 프로그래밍은 객체의 상태를 나타내는 데이터와 상태 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각하는데 이때 객체의 상태 데이터를 프로퍼티, 동작을 메서드라 부름**

---

### 상속과 프로토타입

- **상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말함**
- 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거
  - 중복 제거 -> 코드 재사용
  - 생성자 함수가 생성할 모든 인스턴스가 공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현 없이 상위 객체인 프로토타입의 자산을 공유하여 사용 가능

```js
function Circle(radius) {
  this.radius = radius;
}

// Circle 생성자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유해서 사용할 수 있도록 프로토타입에 추가
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있음
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

// Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
// 프로토타입 Circle.prototype으로부터 getArea 메서드를 상속받음
// => Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유
console.log(circle1.getArea === circle2.getArea); // true

console.log(circle1.getArea()); // 3.14~
console.log(circle2.getArea()); // 12.56~
```

---

### 프로토타입 객체

- 객체 간 상속을 구현하기 위해 사용
- 어떤 객체의 상위 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티를 제공
- 프로토타입을 상속 받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용 가능
- 모든 객체는 하나의 프로토타입을 가지며 모든 프로토타입은 생성자 함수와 연결되어 있음
- `[[Prototype]]` 내부 슬롯에는 직접 접근할 수 없지만, `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근 가능
- 프로토타입은 자신의 `constructor` 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 `prototype` 프로퍼티를 통해 프로토타입에 접근 가능

#### `__proto__` 접근자 프로퍼티

- 모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 `[[Prototype]]` 내부 슬롯에 간접적으로 접근 가능

- `__proto__`는 접근자 프로퍼티
  - 접근자 프로퍼티는 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수 `[[Get]], [[Set]]` 프로퍼티 어트리뷰트로 구성된 프로퍼티
    - `[[Get]]` : 프로토타입 취득
    - `[[Set]]` : 프로토타입 할당
- `__proto__` 접근자 프로퍼티는 상속을 통해 사용

  - `__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 `Object.prototype`의 프로퍼티
  - 모든 객체는 상속을 통해 `Object.prototype.__proto__` 접근자 프로퍼티 사용 가능

  ```js
  const person = { name: "april" };

  // person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
  console.log(person.hasOwnProperty("__proto__")); // false

  // __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
  console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
  // {get: ƒ, set: ƒ, enumerable: false, configurable: true}

  // 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
  console.log({}.__proto__ === Object.prototype); // true
  ```

- `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유

  - 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함

  ```js
  // 1. 1번 코드
  const parent = {};
  const child = {};
  // child의 프로토타입을 parent로 설정
  child.__proto__ = parent;
  // parent의 프로토타입을 child로 설정
  parent.__proto__ = child; // TypeError: Cyclic __proto__ value

  // 2. 2번 코드 사용 권장
  const obj = {};
  const parent = { x: 1 };

  // obj 객체의 프로토타입을 취득
  Object.getPrototypeOf(obj); // obj.__proto__;
  // obj 객체의 프로토타입을 교체
  Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

  console.log(obj.x); // 1
  ```

  - 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 함 => 프로퍼티 검색이 한쪽 방향으로만 흘러가야 함
  - But, 위 코드처럼 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지면 무한 루프에 빠져 에러를 발생 시킴
  - 따라서 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있음

- `__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장 X
  - 모든 객체가 `__proto__` 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문
  - `__proto__` 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우, `Object.getPrototypeOf` 메서드 사용
  - 프로토타입 교체의 경우 `Object.setPrototypeOf` 메서드 사용

#### 함수 객체의 `prototype` 프로퍼티

- 함수 객체만이 소유하는 `prototype` 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴
- 모든 객체가 가지고 있는 `__proto__` 접근자 프로퍼티와 함수 객체만이 가지고 있는 `prototype` 프로퍼티는 결국 동일한 프로토타입을 가리키지만, 프로퍼티를 사용하는 주체가 다름
- `__proto__` 접근자 프로퍼티 : 모든 객체
- `prototype` 프로퍼티 : 생성자 함수

#### 프로토타입의 `constructor` 프로퍼티와 생성자 함수

- `constructor` 프로퍼티는 `prototype` 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킴
- 이 연결은 생성자 함수가 생성될 때 이뤄짐

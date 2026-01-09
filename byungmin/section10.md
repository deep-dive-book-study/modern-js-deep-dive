## 10장 객체 리터럴

### 원시 값과 객체의 비교
1. 원시 값: 변경 불가능한 값 (`Immutable`)
- 정의: 메모리에 한 번 저장된 원시 데이터(숫자, 문자열 등) 자체는 수정할 수 없다.(`read-only`)
- 동작: 값을 변경하려면 기존 데이터를 수정하는 것이 아니라, 새로운 메모리 공간에 새로운 값을 생성하고 변수가 가리키는 주소를 교체(`재할당`)

2. 객체: 변경 가능한 값 (`Mutable`)
- 정의: 객체는 메모리에 생성된 이후에도 객체 자체를 직접 수정할 수 있다.
- 메모리 구조: 변수에는 실제 객체가 아닌, 객체가 위치한 참조 값(메모리 주소)이 저장된다.
- 변경 메커니즘:
  - 객체의 프로퍼티를 추가/수정/삭제할 때, 변수가 가진 참조 값(주소)은 그대로 둔 채 메모리 힙(Heap)에 있는 실제 객체 내부만 변경
  - 따라서 원시 값과 달리 재할당 없이도 객체의 상태를 직접 바꿀 수 있다.

3. 예시
```javascript
var person = {
  name: 'Lee', // 프로퍼티: 객체의 상태를 나타내는 데이터
  age: 20,
  
  // 메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작
  sayHello: function () {
    console.log('Hello! My name is ' + this.name);
  }
};

console.log(typeof person); // object
console.log(person); // {name: "Lee", age: 20, sayHello: ƒ}

// 빈 객체 생성
var empty = {};
console.log(typeof empty); // object
```

### 프로퍼티와 메서드
1. 프로퍼티 (`Property`)
- 정의: 객체의 상태를 나타내는 데이터
- 구성: `프로퍼티 키(Key)`와 `프로퍼티 값(Value)`의 쌍으로 이루어짐
- 키(Key)의 특징:
  - 빈 문자열을 포함한 모든 문자열 또는 심벌(Symbol) 값을 사용 가능
  - 문자열이나 심벌 이외의 값을 키로 사용하면 엔진에 의해 암묵적 타입 변환이 일어나 문자열로 바뀜 (예: 숫자를 입력해도 내부적으로는 문자열 "0", "1"로 저장)
  - 식별자 네이밍 규칙을 따르지 않는 이름은 반드시 따옴표('' 또는 "")로 감싸야 함
```javascript
var person = {
  firstName: 'Gildong', // 식별자 규칙 준수 시 따옴표 생략
  'last-name': 'Hong',  // 하이픈 포함 시 따옴표 필수
  0: 'Hello',           // 숫자 0은 문자열 "0"으로 변환
  true: 'Yes'           // 불리언 true는 문자열 "true"로 변환
};

console.log(person[0]);    // "Hello"
console.log(person['0']);  // "Hello"
```

2. 메서드 (`Method`)
- 정의: 프로퍼티 값이 함수인 경우
- 역할: 객체에 묶여 있는 함수로서 객체 내부의 프로퍼티(상태)를 참조하고 조작하는 동작
```javascript
var circle = {
  radius: 5, 
  
  getDiameter: function () {
    return 2 * this.radius; 
  }
};
```

### 프로퍼티 접근
1. 마침표 표기법 (Dot Notation)
- 방식: `객체.프로퍼티키`
- 특징: 프로퍼티 키가 식별자 네이밍 규칙을 준수할 때만 사용 가능

2. 대괄호 표기법 (Bracket Notation)
- 방식: `객체['프로퍼티키']`
- 핵심: 대괄호 내부의 키는 반드시 따옴표로 감싼 문자열이어야 함 (따옴표 생략 시 해당 이름을 식별자/변수로 인식)
- 반드시 사용해야 하는 상황:
  - 네이밍 규칙 불충분: 키 이름에 하이픈(-), 공백이 포함되거나 숫자로 시작하는 경우
  - 동적 키 접근: 변수에 저장된 값을 통해 프로퍼티를 가져와야 하는 경우
```javascript
var person = { 'last-name': 'Lee' };

// 네이밍 규칙 불충분
// person.last-name; (X) -> 마이너스 연산자로 오해하여 에러 발생
person['last-name']; // (O)

// 동적 키 접근
var key = 'name';
var person = { name: 'Lee' };

person[key]; // (O) 변수 key의 값 'name'으로 접근

// 객체에 존재하지 않는 프로퍼티에 접근하면 undefined 반환
var person = {
    name: 'Lee'
};

console.log(person.age); // undefined
```

### 메서드 축약 표현
1. 문법 비교
- 기존 (ES5): 프로퍼티 값으로 함수 할당 (문법:` 키: function() { ... }`)
- 축약 (ES6): `function` 키워드와 콜론(`:`)을 생략 (문법: `이름() { ... }`)
```javascript
const obj = {
  name: 'Lee',
  // 기존 방식
  sayHi: function() {
    console.log('Hi! ' + this.name);
  },
  // 축약 표현
  sayHello() {
    console.log('Hello! ' + this.name);
  }
};
```

### 메서드 정의 방식에 따른 차이
객체 리터럴 안에서 메서드를 정의하는 방식은 두 가지가 있으며, 엔진은 이를 서로 다르게 취급한다.

1. 프로퍼티에 할당한 메서드 (`f: function() {}`)
- 특징: function 키워드를 사용하여 프로퍼티 값으로 함수를 할당하는 방식
- 생성자 기능: 객체에 속해 있음에도 불구하고 new 연산자와 함께 호출하여 인스턴스를 생성할 수 있음 (Constructor)

2. 축약 표현으로 정의한 메서드 (`f() {}`)
- 특징: function 키워드를 생략하고 선언하는 ES6 방식
- 생성자 기능: 인스턴스를 생성할 수 있는 능력이 아예 제거 (Non-constructor)
  - 따라서 new 호출 시 에러가 발생하며, 불필요한 프로토타입 생성을 하지 않아 메모리 효율이 더 좋음
- 추가 기능: 내부 슬롯 [[HomeObject]]를 가지므로 super 키워드를 사용하여 부모 객체에 접근 가능함
### 17장 생성자 함수에 의한 객체 생성

#### Object 생성자 함수

생성자 함수: new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
인스턴스: 생성자 함수에 의해 생성된 객체

#### 객체 리터럴 vs Object 생성자 함수

##### 객체 리터럴

```js
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  },
};
```

장점: 간편하다
단점: 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야하는 경우 매번 같은 프로퍼티를 기술해야함

##### Object 생성자 함수

```js
function Circle(radius) {
  this.radius=radius
  this.getDiameter=function () {
    return 2 * this.radius;
  },
};
```

단점: 기본적으로 함수이기 때문에, 휴먼 에러가 발생할 가능성
장점: 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성

#### 생성자 함수의 인스턴스 생성 과정

1. 암묵적으로 빈 객체가 생성되고 this에 바인딩(식별자와 값을 연결하는 과정)됨
2. this에 바인딩 되어 있는 인스턴스를 초기화한다
3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다

주의: **명시적으로 객체**를 반환하면 **암묵적인 this 반환이 무시**됨
주의: **명시적으로 원시값**을 반환하면 원시값 변환은 무시되고 **암묵적으로 this가 반환**됨

-> 생성자 함수 내부에서 return문 반드시 생략

#### 내부 메서드 `[[Call]]`과 `[[Constructor]]`

함수는 객체지만 일반 객체와 다름
함수로서 동작하기 위해 `[[Enviroment]]`, `[[FormalParameter]]` 등의 내부 슬롯과 `[[Call]]`과 `[[Constructor]]` 같은 내부 매서드를 추가로 가지고 있음

`[[Call]]` -> 일반 함수로서 호출될 때 호출(callable)
`[[Constructor]]` -> 생성자 함수로서 호출될 때 호출(constructor)

모든 함수는 `callable && (constructor || non-constructor)`

constructor: 함수 선언문, 함수 표현식, 클래스
non-constructor: 메서드(ES6 매서드 축약표현), 화살표 함수

non-constructor인데 new 연산자를 붙여 호출하면 에러
constructor인데 일반 함수처럼 호출 가능 -> 실수 유발 가능성 (이때 this는 window랑 바인딩됨)

#### 생성자 함수가 new 연산자 없이 호출되는 거 방지

1. `new.target`
   생성자 함수로서 호출시 -> 함수 자신
   일반 함수로서 호출시 -> undefined

```js
function Circle(radius) {
  if (!new.target) {
    return new Circle(radius);
  }
  //...
}
```

2. 스코프 세이프 패턴
   IE는 `new.target`을 지원하지 않음

```js
function Circle(radius) {
  if (!(this instanceof Circle)) {
    return new Circle(radius);
  }
  //...
}
```

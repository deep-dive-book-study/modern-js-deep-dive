## 타입 변환과 단축 평가

### 암묵적 타입 변환

- 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 변환되는 것
- 타입 강제 변환이라고도 함

---

### 명시적 타입 변환

- 개발자가 의도적으로 값의 타입을 변환하는 것
- 타입 캐스팅이라고도 함 <br/>

- 문자열 타입으로 변환
  - `String` 생성자 함수를 new 연산자 없이 호출
  - `Object.prototype.toString` 메서드 사용
  - 문자열 연결 연산자 이용
  ```javascript
  // String 생성자 함수를 new 연산자 없이 호출
  String(1); // "1"
  String(NaN); // "NaN"

  // Object.prototype.toString 메서드 사용
  (1).toString(); // "1"
  Infinity.toString(); // "Infinity"

  // 문자열 연결 연산자 이용
  1 + ""; // '1'
  true + ""; // 'true'
  ```
- 숫자 타입으로 변환
  - `Number` 생성자 함수를 new 연산자 없이 호출
  - `parseInt`, `parseFloat` 함수 사용(문자열만 변환 가능)
  - `+` 단항 산술 연산자 이용
  - `*` 산술 연산자 이용
  ```javascript
  // Number 생성자 함수를 new 연산자 없이 호출
  Number("0"); // 0
  Number("-1"); // -1

  // parseInt, parseFloat 함수 사용
  parseInt("0"); // 0
  parseFloat("10.53"); // 10.53

  // + 단항 산술 연산자 이용
  +"0"; // 0
  +"10.35"; // 10.35
  +true; // 1
  +false; // 0

  // * 산술 연산자 이용
  "0" * 1; // 0
  "10.53" * 1; // 10.53
  ```
- 불리언 타입으로 변환
  - `Boolean` 생성자 함수를 new 연산자 없이 호출
  - `!` 부정 논리 연산자를 두 번 사용
  ```javascript
  // Boolean 생성자 함수를 new 연산자 없이 호출
  Boolean("x"); // true
  Boolean(0); // false

  // ! 부정 논리 연산자 두 번 사용
  !!"x"; // ture
  !!"false"; // true
  !!0; // false
  ```

### truthy / falsy한 값이 무엇인가?

- 불리언 타입이 아닌 값을 `Truthy` 값 또는 `Falsy` 값으로 구분
- `Truthy`
  - 참으로 평가되는 값
  - `true`
  - Falsy 값을 제외한 모든 값
- `Falsy`
  - 거짓으로 평가되는 값
  - `false`
  - `undefined`
  - `null`
  - `0`, `-0`
  - `NaN`
  - `‘ ‘`

---

### 단축 평가

- 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 의미
- `if문` 대체 가능
- 논리 연산자
  - `논리곱(&&)`
    - 두 개의 피연산자가 모두 `true`로 평가될 때 `true` 반환
    - 왼쪽이 `falsy` 값인 것을 확인하는 순간 즉시 평가를 멈춘 두 그 값을 그대로 반환
    ```javascript
    ‘Cat’ && ‘Dog’; // 'Dog'
    // Cat과 Dog 모두 falsy 값이 아니기 때문에 논리 연산의 결과를 결정하는
    // 두 번째 피연산자 반환

    var elem = null; // null은 falsy 값
    var value = elem && elem.value; // null
    // elem은 null로 falsy 값이기 때문에 평가를 멈춘 후 바로 elem의 값인 null 반환
    ```
  - `논리합(||)`
    - 두 개의 피연산자 중 하나만 `true`로 평가되어도 `true` 반환
    - ‘Cat’ || ‘Dog’ ⇒ ‘Cat’
- 옵셔널 체이닝 연산자
  - `?.` 연산자는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 `undefined` 반환, 그렇지 않으면 우항의 프로퍼티 참조를 이어감
  ```javascript
  var elem = null;
  var value = elem?.value;
  console.log(value); // undefined
  ```
- null 병합 연산자
  - `??` 연산자는 좌항의 피연산자가 `null` 또는 `undefined`인 경우 우항의 피연산자를 반환, 그렇지 않으면 좌항의 피연산자 반환
  ```javascript
  var foo = null ?? "default string";
  console.log(foo); // default string
  // 좌항의 피연산자가 null 이기 때문에 우항의 피연산자 반환

  var foo = "" ?? "default string";
  console.log(foo); // ''
  // 좌항의 피연산자가 falsy 값이지만 null 또는 undefined가 아니기 때문에 좌항의
  // 피연산자 반환
  ```

## 원시 값과 객체 비교

- 자바스크립트는 원시 타입과 객체 타입으로 구분
- 원시 타입
  - 원시 값
    - 변경 불가능한 값으로 읽기 전용
    - 데이터(값) 자체의 속성
    - 변수에 할당 시 변수에는 실제 값 저장
    - 데이터 신뢰성 보장 ⇒ 불변성
    ```javascript
    let hello = "안녕";
    hello = hello + "하세요";

    console.log(hello); // 안녕하세요

    // 안녕이라는 원시 값을 hello라는 변수에 할당
    // hello 변수에 안녕 + 하세요 연산이 실행돼 새로운 값인 안녕하세요를 재할당
    // 여기서 원래 메모리에 있던 안녕이라는 값은 변경되지 않았음
    // hello라는 변수가 다른 값을 참조하고 있을 뿐 데이터는 변경 X
    // 이것이 원시 값은 변경 불가능하다는 의미임

    // 문자열은 유사 배열 객체이므로 배열과 유사하게 인덱스로 접근 가능
    // But, 원시 값이므로 변경 불가 => 재할당을 통해서만 변경 가능
    let str = "안녕";
    str[0] = "가";

    console.log(str); // 안녕
    ```
- 객체 타입
  - 변경 가능한 값
  - 변수에 할당 시 참조 값 저장
  - 얕은 복사
    - 한 단계까지만 복사
    - 원본 객체와 다름
    ```javascript
    const o = { x: { y: 1 } };

    const c1 = { ...o };
    // 스프레드 문법으로 객체 o 복사
    // 스프레드 문법은 객체의 1단계 프로퍼티들만 복사하고, 그 프로퍼티의 값은 그대로 가져옴
    console.log(c1 === o); // false
    console.log(c1.x === o.x); // true

    // o 객체는 객체가 중첩되어 있는 구조
    // c1은 스프레드 문법으로 o 객체를 복사
    // 객체를 복사했지만 껍데기가 다른 완전히 다른 객체이기 때문에 c1 === o는 false
    // 하지만 두 객체의 속성은 x로 같고, 얕은 복사로 인해 동일한 메모리 주소 참조
    // 따라서 c1.x === o.x는 true
    ```
  - 깊은 복사
    - 객체에 중첩되어 있는 객체까지 모두 복사
    - 원본 객체와 다름
    ```javascript
    // structuredClone
    const o = {x : {y : 1}};

    const c2 = structuredClone(o);

    console.log(c2 === o); // false
    console.log(c2.x === o.x); // false => 내부 객체도 새로 복사

    // JSON.stringify(), JSON.parse()
    const o = {x : y : 1}};

    const c3 = JSON.parse(JSON.stringify(o));

    console.log(c3 === o); // false
    console.log(c3.x === o.x); // false => 내부 객체도 새로 복사
    ```

---

### 값에 의한 전달

- 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달되는데 이를 값에 의한 전달이라고 함
- 값을 전달하는 것이 아닌 메모리 주소를 전달하는 것으로 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있음

```javascript
var score = 90;

var copy = score;

console.log(score, copy); // 90 90
console.log(score === copy); // true

score = 100;

console.log(score, copy); // 100 90
console.log(score === copy); // false
```

---

### 참조에 의한 전달

- 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달되는데 이를 참조에 의한 전달이라고 함
- 두 개의 식별자가 하나의 객체 공유

```javascript
var person = {
  name: " Kim",
};

var copy = person;

console.log(copy === person); // true => 동일한 객체 참조(메모리 주소 동일)

copy.name = "Lee";

person.address = "Seoul";

console.log(person); // {name : Lee, address : Seoul}
console.log(copy); // {name : Lee, address : Seoul}
```

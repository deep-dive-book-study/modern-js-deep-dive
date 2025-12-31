## 15장 let, const 키워드와 블록 레벨 스코프

### let, const의 호이스팅 메커니즘
1. 선언 단계와 초기화 단계의 분리
- var 키워드는 `선언과 초기화(undefined)`가 한꺼번에 일어나지만, let과 const는 이 두 단계가 분리되어 진행된다.
  - 선언 단계 (Declaration Phase): 런타임 이전 평가 단계에서 식별자를 실행 컨텍스트에 등록한다. (호이스팅 발생)
  - 초기화 단계 (Initialization Phase): 실제 소스코드에서 선언문에 도달했을 때 메모리 공간을 확보하고 undefined를 할당한다.

2. 일시적 사각지대 (TDZ, Temporal Dead Zone)
- 스코프의 시작 지점부터 초기화 단계 시작 지점(선언문)까지 변수를 참조할 수 없는 구간을 TDZ라고 부른다.
  - `let`과 `const`도 런타임 이전에 선언은 되어 있지만, 초기화가 되지 않은 상태이므로 메모리에 값이 없다.
  - 이 상태에서 변수에 접근하려 하면 엔진은 `ReferenceError`를 발생시킨다.
```javascript
// 1. 런타임 이전: 식별자 foo가 선언되지만 초기화는 되지 않음. (TDZ 시작)

console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
// ^-- TDZ 구간에서 참조했기 때문에 에러가 발생한다.

let foo; // 2. 초기화 단계: 여기서 비로소 undefined가 할당된다. (TDZ 종료)

console.log(foo); // undefined

foo = 1; // 3. 할당 단계: 1이 할당된다.
console.log(foo); // 1
```
3. 호이스팅이 발생한다고 하는 이유
```javascript
let x = 1; // 전역 변수

{
    console.log(x); // ReferenceError: Cannot access 'x' before initialization
    let x = 2; // 지역 변수
}
```

### 전역 객체와 let
1. 전역 객체(Global Object)란?
- 정의: 코드가 실행되기 전 자바스크립트 엔진에 의해 가장 먼저 생성되는 특수한 객체
- 환경별 이름: 브라우저 환경에서는 window, Node.js 환경에서는 global이라고 부른다. (ES11 통합 명칭은 globalThis)
- 특징: 전역 변수나 전역 함수, 그리고 표준 빌트인 객체(Object, Array, String 등)를 프로퍼티로 갖는다.

2. `var`와 전역 객체의 관계
- `var` 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 된다. 
```javascript
var x = 1;

// var로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체의 프로퍼티는 식별자처럼 참조할 수 있다.
console.log(x); // 1
```

3. `let/const`와 전역 객체의 관계
- let이나 const로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉, window.x와 같은 방식으로 접근할 수 없다.
- 저장 위치: let/const 전역 변수는 전역 객체가 아니라, 개념적인 블록인 보이지 않는 개념적 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.
- 이유: 전역 객체에 모든 변수를 붙이면 네임스페이스가 오염되고 관리하기 어렵기 때문에, 현대적인 방식인 let/const는 이를 분리하여 관리한다.
```javascript
let y = 10;

// let으로 선언한 전역 변수는 전역 객체 window에 속하지 않는다.
console.log(window.y); // undefined
console.log(y); // 10
```

4. 왜 `let`은 전역 객체에 들어가지 않는가?
- 네임스페이스 보호: 전역 객체는 브라우저의 경우 창(Window) 제어와 관련된 수많은 표준 프로퍼티를 가지고 있다. 사용자가 만든 변수가 이들과 충돌하는 것을 방지한다.
- 보안 및 캡슐화: 전역 객체를 통해 어디서든 쉽게 접근하고 수정하는 것을 막아 코드의 안전성을 높인다.
- 엄격한 스코프 관리: let은 블록 레벨 스코프를 지원하므로, 엔진 내부에서 이를 별도의 선언적 환경 레코드에서 관리하는 것이 일관성 면에서 효율적이다.
### 연산자

- **산술 연산자**
  - `+, -, *, /, %`
    ```javascript
    5 + 2; // 7
    5 - 2; // 3
    5 * 2; // 10
    5 / 2; // 2.5
    5 % 2; // 1
    ```
    - **증감 연산자**
      - `++, --` : 1을 더하거나 빼는 연산자
        ```javascript
        let x = 5,
          result;

        result = x++; // 선할당 후증가, 후위 연산
        console.log(result, x); // 5 6

        result = ++x; // 선증가 후할당, 전위 연산
        console.log(result, x); // 7 7

        result = x--; // 선할당 후감소, 후위 연산
        console.log(result, x); // 7 6

        result = --x; // 선감소 후할당, 전위 연산
        console.log(result, x); // 5 5
        ```
- **대입 연산자**
  - 변수에 특정 값을 대입하는 역할
  - `=, +=, -=, *=, /=, %=`
    ```javascript
    let x;

    x = 10;
    console.log(x); // 10

    x += 5; // x = x + 5
    console.log(x); // 15

    x -= 5; // x = x - 5
    console.log(x); // 10

    x *= 5; // x = x * 5
    console.log(x); // 50

    x /= 5; // x = x / 5
    console.log(x); // 10

    x %= 5; // x = x % 5
    console.log(x); // 0
    ```
- **비교 연산자**
  - 두 개의 값을 비교할 때 사용되는 연산자
  - **동등 / 일치 비교 연산자**
    - `==, ===, !=, !==`
    ```javascript
    let num1 = 10;
    let num2 = "10";

    console.log(num1 === num2); // false
    // 일치 비교 연산자 => 두 변수의 값과 자료형이 같은지 비교
    // number 타입과 string 타입이기 때문에 false

    console.log(num1 == num2); // true
    // 동등 비교 연산자 => 두 변수의 값이 일치하는지 비교
    // 두 변수의 값이 10으로 동일하기 때문에 true

    console.log(num1 !== num2); // true
    // 불일치 비교 연산자 => 두 변수의 값과 자료형이 같은지 비교
    // number 타입과 string 타입이기 때문에 일치하지 않으므로 true

    console.log(num1 != num2); // false
    // 부동등 비교 연산자 => 두 변수의 값이 일치하는지 비교
    // 두 변수의 값이 10으로 동일하기 때문에 false
    ```
  - **대소 관계 비교 연산자**
    - `>, <, >=, <=`
    ```javascript
    let a = 10;
    let b = 20;
    let c = 10;

    console.log(a < b); // true
    console.log(a > b); // false
    console.log(b >= c); // true
    console.log(b > c); // true
    console.log(a <= c); // true
    console.log(a > c); // false
    ```
- **연결 연산자**
  - 문자열과 문자열 혹은 다른 자료형을 합쳐서 하나의 문자열로 만드는 연산자
  - `+`
  ```javascript
  let price = 10000;

  console.log("가격 : " + price + "원"); // 가격 : 10000원
  // 숫자 + 문자열 => 문자열
  ```
- **논리 연산자**
  - true와 false 값으로 이루어진 Boolean 타입을 위한 연산자로, 조건 확인 시 사용
  - `||, &&, !`
  ```javascript
  // 논리합(||)
  true || false; // true
  true || true; // true
  false || false; // false
  false || true; // true

  // 논리곱(&&)
  true && false; // false
  true && true; // true
  false && false; // false
  false && true; // false

  // 논리 부정(!)
  !true; // false
  !false; // true
  ```
- **null 병합 연산자**
  - 왼쪽 값이 null이거나 undefined일 경우 기호의 오른쪽에 있는 값을 반환하고, 왼쪽의 값이 null이나 undefined이 아니라면 왼쪽의 값을 반환하는 연산자
  - `??`
  - 주로 변수에 기본값을 할당하고 싶을 때 사용
  ```javascript
  let num1;
  let num2 = 10;

  console.log(num1 ?? 20); // 20
  console.log(num2 ?? 20); // 10
  ```
- **삼항 연산자**
  - A라는 조건이 true라면 B를, false라면 C를 반환하는 연산자
  - `A ? B : C`
  - `if`문을 대체해서 자주 사용
  ```javascript
  let num = 100;
  let result = num % 2 === 0 ? "짝수" : "홀수";

  console.log(result);
  ```
- **typeof 연산자**
  - string, number, boolean, undefined, symbol, object, function 중 하나 반환
  - null 반환 X
  - null 값 연산 시 object 반환
    - `===` 일치 연산자 이용하여 확인
  - 선언하지 않은 식별자 연산 시 undefined 반환

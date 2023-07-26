# 20 strict mode

## 20.1 strict mode란?

```javascript
function foo() {
  x = 10;
}
foo();
console.log(x);
```

- const, let, var이 없어 오류를 발생시킬꺼 같지만 암묵적으로 전역 객체 x 프로퍼티를 동적 생성하여 전역 변수 같이 사용 가능
- 이러한 현상이 암묵적 전역
- 암묵적 전역과 같은 잠재적 오류의 발생을 막기 위해 strict mode(엄격 모드)가 추가
- 오류 발생 확률이 높거나 자바스크립트 엔진의 최적화에 문제를 일으킬 코드에 대한 명시적 에러 발생
- ESLint와 같은 린트 도구를 사용하여 strict mode와 유사한 효과
- 문법적 오류 뿐 아니라 잠재적 오류까지 찾아 오류 원인을 리포팅

## 20.2 strict mode의 적용

- strict mode 적용시 전역 선두 또는 함수 몸체에 'use strict';를 추가
- 전역 선두에 추가시 스크립트 전체 strict mode 적용

```javascript
"use strict";

function foo() {
  x = 10;
}
foo();
```

## 20.3 전역 strict mode를 적용하는 것 피하자

- 전역에 strict mode 적용은 바람직 하지 않음
- 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하여 strict mode 적용

```javascript
(function () {
  "use strict";
})();
```

## 20.4 함수 단위로 strict mode를 적용 피하자

- strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직

```javascript
(function () {
  var let = 10;
  function foo() {
    "use strict";
    let = 20;
  }
  foo();
})();
```

## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

- 선언하지 않는 변수 참조시 ReferenceError 발생

```javascript
(function () {
  "use strict";
  x = 1;
  console.log(x);
})();
```

### 20.5.2 변수, 함수, 매개변수의 삭제

- delete 연산자로 변수, 함수, 매개변수 삭제시 SyntaxError 발생

```javascript
(function () {
  "use strict";
  var x = 1;
  delete x;

  function foo(a) {
    delete a;
  }
  delete foo;
})();
```

### 20.5.3 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용시 SyntaxError 발생

```javascript
(function () {
  "use strict";

  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

### 20.5.4 with 문의 사용

- with 문 사용시 SyntaxError 발생 / with 문은 전달된 객체를 스코프 체인에 추가하는 것으로 동일한 객체의 프로퍼티를 반복 사용할 때 객체 이름 생략 가능하여 코드 선능과 가독성이 나빠지는 문제

```javascript
(function () {
  "use strict";

  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

- strict mode에서 함수를 일반 함수로 호출 시 this에 undefined 바인딩
- 생성자 함수가 아닌 일반 함수 내부에서는 this 사용 필요 x

```javascript
(function () {
  "use strict";

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); //Foo
  }
  new Foo();
})();
```

### 20.6.2 arguments 객체

- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체 반영 x

```javascript
(function (a) {
  "use strict";
  a = 2;

  console.log(arguments);
})(1);
```

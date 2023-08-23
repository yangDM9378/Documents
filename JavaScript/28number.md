# 28 Number

## 28.1 Number 생성자 함수

- Number 객체는 생성자 함수 객체
- new 연산자와 함께 호출하여 Number 인스턴스 생성
- new 연산자와 함께 호출시 [[NumberData]] 내부 슬롯에 0을 할당한 Number 래퍼 객체 생성

```javascript
const numObj = new Number();
```

## 28.2 Number 프로퍼티

### 28.2.1 Number.EPSILON

- 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이
- 부동 소수점으로 인해 발생하는 오차 해결

```javascript
function isEqual(a, b) {
  return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3);
```

### 28.2.2 Number.MAX_VALUE

- 자바스크립트에서 표현 가능한 가장 큰 양수 값
- 1.7976931348623157 \* 10^308

### 28.2.3 Number.MIN_VALUE

- 자바스크립트에서 표현 가능한 가장 작은 양수 값
- 5\*10^-324

### 28.2.4 Number.MAX_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현 가능한 가장 큰 정수 값
- 9007199254740991

### 28.2.5 Number.MIN_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현 가능한 가장 작은 정수 값
- -9007199254740991

### 28.2.6 Number.POSITIVE_INFINITY

- 양의 무한대를 나타내는 숫자

### 28.2.7 Number.NEGATIVE_INFINETY

- 음의 무한대를 나나태는 숫자

### 28.2.8 NUMBER.NaN

- 숫자가 아닌을 나타내는 숫자값

## 28.3 Number 메서드

### 28.3.1 Number.isFinite

- 인수로 전달된 숫자값이 정상적인 유한수 인지 검사

### 28.3.2 Number.isInteger

- 인수로 전달된 숫자값이 정수인지 검사

### 28.3.3 Number.isNaN

- 인수로 전달된 숫자값이 NaN인지 검사

### 28.3.4 Number.isSafeInteger

- 인수로 전달된 숫자값이 안전한 정수인지 검사

### 28.3.5 Number.prototype.toExponential

- 숫자를 지수 표기법으로 반환하여 문자열 반환

### 28.3.6 Number.prototype.toFixed

- 숫자를 반올림하여 문자열로 반환

### 28.3.7 Number.prototype.toPrecision

- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 수를 반올림

### 28.3.8 Number.prototype.toString

- 숫자를 문자열로 반환

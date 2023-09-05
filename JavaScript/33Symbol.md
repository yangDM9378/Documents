# 33.7번째 데이터 타입 Symbol

## 33.1 심벌이란?

- ES6에서 도입된 변경 불가능한 원시 타입의 값
- 중복되지 않는 유일한 값
- 유일한 프로퍼티 키를 만들기 위해 사용

## 33.2 실벌 값의 생성

### 33.2.1 Symbol 함수

- 심벌 값은 Symbol 함수를 호출하여 생성

```javascript
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol
console.log(mySymbol); // Symbol()
```

- Symbol 함수에는 선택적 문자열을 인수로 전달
- 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며, 심벌 값 생성에 영향을 주지 않음

```javascript
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");
console.log(mySymbol1 == mySymbol2); // false
```

- 객체처럼 접근시 암묵적으로 래퍼 객체를 생성

```javascript
const mySymbol = Symbol("mySymbol");
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 불리언 타입을 제외한 다른 타입으로 암묵적 형변환 불가능

### 33.2.2 Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출

## 33.3 심벌과 상수

- 특별한 의미가 없고 상수 이름 자체의 의미가 있는 경우 상수 값이 변경되는 것을 방지하고 중복을 제거하기 위해 Symbol을 사용

```javascript
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};

const myDirection = Direction.UP;

if (myDirection == Direction.up) {
  console.log("You are going UP");
}
```

## 33.4 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들며, 동적으로 생성 가능

```javascript
const obj = {
  [Symbol.for("mySymvol")]: 1,
};

obj[Symbol.for("mySymbol")];
```

- 심벌 값은 유일무이한 값으로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절때 충돌 x

## 33.5 심벌과 프로퍼티 은닉

- 외부에 노출할 필요가 없는 프로퍼티 은닉

```javascript
const obj = {
  [Symbol("mySymbol")]: 1,
};
for (const key in obj) {
  console.log(key); // 출력되지 않음
}
console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

## 33.6 심벌과 표준 빌트인 객체 확장

- 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장x
- 표준 사양에 존재하는 메서드와 이름이 중복될 가능성이 있기 때문
- Symbol을 사용하면 다른 프로퍼티 키와 충돌이 발생하지 않음

## 33.7 Well-known Symbol

- 자바스크립트가 제공하는 빌트인 심벌 값을 Well-known Symbol이라 부름

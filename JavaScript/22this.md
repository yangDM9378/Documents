# 22 this

## 21.1 this 키워드

- 동작을 나타내는 메서드는 자신이 속한 객체의 상태, 프로퍼티를 참조하고 변경 가능 해야함
- 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 자신이 속한 객체를 가리키는 식별자를 참조 가능 해야함
- 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 자신의 객체를 가리키는 식별자를 재귀적으로 참조

```javascript
const circle = {
  // 프로퍼티: 객체고유 상태 데이터
  radius: 5,
  //메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 자신이 속한 객체의 프로퍼티나 다른 메서드 참조시
    // 자신이 속한 객체인 circle을 참조해야함
    return 2 * circle.radius;
  },
};
```

- 생성자 함수 방식으로 인스턴스 생성

```javascript
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없음
  ????.radius = radius;

}
Circle.prototype.getDiameter = function {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없음
  return * 2 ????.radius;
}

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수 정의
const circle = new Circle(5);
```

- this : 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수 / 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조 가능
- this 바인딩은 함수 호출 방식에 의해 동적으로 결정

```javascript
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체
    return 2 * this.radius;
  },
};
```

```javascript
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스
  this.radius = radius;

}
Circle.prototype.getDiameter = function {
  // this는 생성자 함수가 생성할 인스턴스
  return * 2 this.radius;
}
```

- 전역에서 this는 전역 객체 window를 가리킴
- 일반 함수 내부의 this는 전역 객체 window를 가리킴
- 메서드 내부에서 this는 메서드를 호출한 객체를 가리킴
- 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킴

## 22.2 함수 호출 방식과 this 바인딩

- this 바인딩 : 함수가 어떻게 호출되었는지에 따라 동적으로 결정

### 22.2.1 일반 함수 호출

- 기본적으로 this에는 전역 객체 바인딩 -전역 함수 뿐 아니라 중첩 함수를 일반 함수로 호출 시 this는 전역 객체

```javascript
function foo() {
  console.log("foo", this); // window
  function bar() {
    console.log("bar", this); // window
  }
  bar();
}
foo();
```

- strict mode 적용 시 undefined 바인딩

```javascript
function foo() {
  "use strict";
  console.log("foo", this); // window
  function bar() {
    console.log("bar", this); // window
  }
  bar();
}
foo();
```

- 콜백 함수 내부의 this에도 전역 객체 바인딩

```javascript
var value = 1;
const obj = {
  value: 100,
  foo() {
    console.log(this); // {value:100, foo:f}
    // 콜백 함수 내부의 this에는전역 객체 바인딩
    setTimeout(function () {
      console.log(this); // window
      console.log(this.value); // 1
    }, 100);
  },
};
obj.foo();
```

- 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드 this 바인딩과 일치 시키기

```javascript
var value = 1;
const obj = {
  value: 100,
  foo() {
    const that = this;
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  },
};
obj.foo();
```

- 명시적 this 바인딩 : Function.prototype.apply/call/bind 메서드 사용

```javascript
var value = 1;
const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this 바인딩
    setTimeout(
      function () {
        console.log(this.value); // 100
      }.bind(this),
      100
    );
  },
};
```

- 화살표 함수 사용

```javascript
var value = 1;
const obj = {
  value = 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this
    setTimeout(() => console.log(this.value),100)
  }
}
```

### 22.2.2 메서드 호출

- 메서드 내부의 this는 메서드 호출 객체, 메서드 이름 앞의 마침표. 연산자 앞의 객체가 바인딩

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person("Lee");

// getName 메서드를 호출한 객체 me
console.log(me.getName()); // Lee

Person.prototype.name = "Kim";

// getName 메서드 호출 객체 Person.prototype
console.log(Person.prototype.getName()); // Kim
```

### 22.2.3 생성자 함수 호출

- 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스 바인딩
- new 연산자가 없다면 일반 함수로 동작

```javascript
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);
const circle3 = Circle(15);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
console.log(circle3); // undefined
```

### 22.2.4 Function.prototype.apply/call/bind

- apply와 call 메서드는 함수를 호출 / 첫번째 인수로 전달한 객체를 호출하여 this에 바인딩

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window
console.log(getThisBinding().apply(thisArg)); //{a:1}
console.log(getThisBinding().call(thisArg)); //{a:1}
```

- bind 메서드는 함수를 호출 x / 첫번쨰 인수로 전달한 값으로 this 바인딩이 교체된 함수 생성

```javascript
const person = {
  name: "Lee",
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(this.name); // Lee
});
```

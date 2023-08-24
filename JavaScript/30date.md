# 30 Date

- Date는 날짜 시간을 위한 메서드를 제공하는 빌트인 객체

## 30.1 Date 생성자 함수

- Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가짐

### 30.1.1 new Date()

- new 연산자와 함께 호출 시 현재 날짜와 시간을 가지는 Date 객체를 반환
- Date 생성자 함수를 new 연산자 없이 호출시 Date 객체를 반환하지 않고 날짜와 시간 정보를 문자열 반환

### 30.1.2 new Date(milliseconds)

- Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달시 1970년 1월 1일 00:00:00을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체 반환

### 30.1.3 new Date(dateString)

- Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달 시 지정된 날짜와 시간을 나타내는 Date 객체를 반환

```javascript
new Date("May 26, 2020 10:00:00");
// -> Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)

new Date("2020/03/26/10:00:00");
// -> Tue May 26 2020 10:00:00 GMT+0900 (대한민국 표준시)
```

### 30.1.4 new Date(year, month[,day, hour,minute,second,millisecond])

- Date 생성자 함수에 연,월,일,시,분,초,밀리초를 의미하는 숫자를 인수로 전달 시 지정된 날짜와 시간을 나타내는 Date 객체 반환

## 30.2 Date 메서드

### 30.2.1 Date.now

- 현재 시간까지 경과한 밀리초를 숫자로 반환

### 30.2.2 Date.parse

- 인수로 전달된 지정 시간의 인수와 동일한 형식까지 밀리초로 반환

### 30.2.3 Date.UTC

- 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환

### 30.2.4 Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수를 반환

```javascript
new Date("2020/07/24").getFullYear(); //-> 2020
```

### 30.2.5 Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수를 설정, 및 월,일도 설정 가능

```javascript
const today = new Date();

// 년도 지정
today.setFullYear(2000);
today.getFullYear(); // -> 2000

// 년도/월/일 지정
today.setFullYear(1900, 0, 1);
today.getFullYear(); // -> 1900
```

### 30.2.6 Date.prototype.getMonth

- Date 객체의 월을 나타내는 0~11의 정수를 반환

```javascript
new Date("2020/07/24").getMonth(); // -> 6
```

### 30.2.7 Date.prototype.setMonth

- Date 객체의 월을 나나태는 0~11의 정수를 설정

### 30.2.8 Date.prototype.getDate

- Date 객체의 날짜(1~31)를 나타내는 정수를 반환

### 30.2.9 Date.prototype.setDate

- Date 객체의 날짜를 나타내는 정수를 설정

### 30.2.10 Date.prototype.getDay

- Date 객체의 요일(0~6)을 나타내는 정수를 반환
- 일요일이 0 토요일이 6

### 30.2.11 Date.prototype.getHours

- Date 객체의 시간을 나타내는 정수 반환

### 30.2.12 Date.prototype.setHours

- Date 객체의 시간을 나타내는 정수 설정

### 30.2.13 Date.prototype.getMinutes

- Date 객체의 분을 나타내는 정수 반환

### 30.2.14 Date.prototype.setMinutes

- Date 객체의 분을 나타내는 정수 설정

### 30.2.15 Date.prototype.getSeconds

- Date 객체의 초을 나타내는 정수 반환

### 30.2.16 Date.prototype.setSeconds

- Date 객체의 초을 나타내는 정수 설정

### 30.2.17 Date.prototype.getMilliseconds

- Date 객체의 밀리초을 나타내는 정수 반환

### 30.2.18 Date.prototype.setMilliseconds

- Date 객체의 밀리초을 나타내는 정수 설정

### 30.2.19 Date.prototype.getTime

- Date 객체의 시간까지 경과된 밀리초 반환

### 30.2.20 Date.prototype.setTime

- Date 객체의 시간까지 경과된 밀리초 설정

### 30.2.21 Date.prototype.getTimezoneOffset

- Date 객체에 지정된 로캘 시간과의 차이를 분 단위로 반환

### 30.2.22 Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜 반환

### 30.2.23 Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열 반환

### 30.2.24 Date.prototype.toISOString

- ISO 8601형식으로 Date 객체의 날짜와 시간을 표현한 문자열 반환

### 30.2.25 Date.prototype.toLocaleString

- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열 반환

### 30.2.26 Date.prototype.toLocaleTimeString

- 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열 반환

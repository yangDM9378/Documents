# 32 String

## 32.1 String 생성자 함수

- new 연산자와 함께 호출하여 String 인스턴스를 생성 가능
- new 연산자와 함께 호출시 [[StringData]] 내부 슬롯에 빈 문자열 할당한 String 래퍼 객체 생성

```javascript
const strObj = new String("Lee");
console.log(strObj);
// String {0:"L", 1:'e', 2:'e', length: 3, [[PrimitiveValue]]:'Lee'
```

- 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블
- 문자열은 원시 값으로 변경 불가, 에러는 발생 하지 않음
- 문자열이 아닌 값을 전달시 문자열로 강제 형 변환 후 할당

## 32.2 length 프로퍼티

- 문자열의 문자 개수 반환

## 32.3 String 메서드

- String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 존재하지 않음
- 항상 새로운 문자열 반환, 문자열은 변경 불가능한 원시 값이기 때문, 읽기 전용 객체로 제공

### 32.3.1 String.prototype.indexOf

- 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환
- String.prototype.includes 메서드를 사용하여 가독성 높게 내부에 해당 문자가 있는지 확인 가능

### 32.3.2 String.prototype.search

- 정규표현식과 매치되는 문자열을 검색하여 인덱스 반환

### 32.3.3 String.prototype.includes

- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인 결과를 boolean값으로 반환

### 32.3.4 String.prototype.startsWith

- 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인

### 32.3.5 String.prototype.endsWith

- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인

### 32.3.6 String.prototype.charAt

- 인수로 전달받은 인덱스에 위치한 문자를 반환

### 32.3.7 String.prototype.substring

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두번째 인수로 전달받은 인덱스에 위치하는 문자의 이전 문자까지 반환

### 32.3.8 String.prototype.slice

- substring과 같게 동작

### 32.3.9 String.prototype.toUpperCase

- 대상 문자열을 모두 대문자로 변경한 문자열 반환

### 32.3.10 String.prototype.toLowerCase

- 대상 문자열을 모두 소문자로 변경한 문자열 반환

### 32.3.11 String.prototype.trim

- 문자열 앞뒤에 공백 문자가 있을 경우 제거한 문자열 반환

### 32.3.12 String.prototype.repeat

- 인수로 받은 정수만큼 반복해 연결한 새로운 문자열 반환

### 32.3.13 String.prototype.replace

- 대상 문자열에서 첫번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두번째 인수로 전달한 문자열로 치환한 문자열 반환

### 32.3.14 String.prototype.split

- 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열 반환

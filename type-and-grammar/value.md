# 값

### 배열
> 어떤 타입의 값이라도 담을 수 있는 그릇<br>
> 배열 크기는 미리 정하지 않고도 선언<br>
- delete 연산자는 `slot`을 제거할 수 있지만, `length`는 유지된다.
- 배열 인덱스는 숫자인데 배열 자체도 하나의 객체여서 `key`/`property` 추가가 가능하다.(`length`는 증가하지 않음)

<br>

### 구멍 난(sparse) 배열
```javascript
var a = [];

a[0] = 0;
// a[1] = 1
a[2] = 2;

console.log(a.length) // 3
```
<br>

### 10진 key 배열
```javascript
var a = [];

a["3"] = 3;

console.log(a.length); // 4
```
- 배열 원소의 인덱스는 확실히 숫자만 쓴다.
- `key`/`property`에 문자열을 쓰고 싶다면, 객체를 쓰자!

<br>

### 유사 배열
```javascript
// Shallow Copy - ES6부터 비권장 Deprecated
var arr = Array.prototype.slice.call(arguments);

// ES6
var arr = Array.from(arguments);
```

<br>

### 문자열 vs 배열
- 문자열
  - 불변(Immutable)
  - `a.charAt(1)` // IE7까지 `a[1]` 값은 `undefined`
  - 문자열 메서드는 항상 새로운 문자열을 생성한 후 반환한다.
- 배열
  - 가변(Mutable)
  - `reverse()` 메서드 존재
- 문자열 <-> 배열
  - `split("")`
  - `join("")`
  
<br>

### 숫자
- number
- IEEE 754(표준부동 소수점 표준) - 배정도 Double Precision 표준 포맷(64비트 바이너리)
- 메서드 숫자 리터럴에 바로 접근
```javascript
// true
0.1 === .1;
1.0 === 1.;

// toExponential()
var exponentialNumber = 1E10;
console.log(exponentialNumber); // 10000000000
exponentialNumber.toExponential(); // 1e+10

// toFixed()
var fixedNumber = 3.1415;
fixedNumber.toFixed(0); // 3
fixedNumber.toFixed(3); // 3.142
fixedNumber.toFixed(5); // 3.14150
 
// toPrecision()
var precisionNumber = 33.1415;
precisionNumber.toPrecision(1); // 3e+1
precisionNumber.toPrecision(2); // 33
precisionNumber.toPrecision(7); // 33.14150
```
- 숫자 뒤 `.` 리터럴로 인식 하므로 주의
```javascript
// Wrong
3.toFixed(3); // SyntaxError

// Good
(3).toFixed(3);
0.3.toFixed(3);
3..toFixed(3);
```
- 진법
```javascript
0b-
0o-
0x-
```
- 오차<br>
> 반올림 오차: 허용 공차(Tolerance)<br>
> 미세한 오차를 머신 입실론(Machine Epsilon)
```javascript
0.1 + 0.2 === 0.3; // false

// ES6~: Number.EPSILON
if(!Number.EPSILON) {
	Number.EPSILON = Math.pow(2, -52);
}

// 동등함(Equality)
var n1 = 0.1 + 0.2;
var n2 = 0.3;

var isEuality = (Math.abs(n1 - n2) < Number.EPSILON);
```

- 크기 & 범위
  - 최댓값: `1.798e+308` === `Number.MAX_VALUE`
  - 최솟값: `5e-324` === `Number.MIN_VALUE`
  - 안전한 범위 (ES6): 32bit 숫자
    - `Number.MAX_SAFE_INTEGER` // 21억
    - `Number.MIN_SAFE_INTEGER` // -21억
    - `Number.isSafeInteger()`
    > 64bit 숫자는 (보내고 받을때) 정확하게 표시할 수 없다.<br>
    `NaN`, `Infinity` 등의 특수 값은 32bit 에서 안전하지 않다.
    `ToInt32()`로 `+0`으로 만든다.
  - 안전하지 않은 값은 string으로 저장해야한다.
  
<br>

### 특수값

> 긍정오류(false positive): 정상을 비정상으로 판단.<br>
> 부정오류(false negative): 비정상을 정상으로 판단.<br>

- `null`
  - `null`은 빈 값이다.
  - `null`은 예전에 값이 있었지만 지금은 없는 상태다.

- `undefined`
  - `undefined`는 실종된(Missing) 값이다.
  - `undefined`는 값을 아직 가지지 않은 것이다.

- `void`
  - 어떤 값이든 무효로 만들어 버린다.
  - `void 0`
  - `return void setTimeout(do, 1000);`
  
- `NaN`(Not a Number)
  > 숫자 아님 보다는,<br>
  > 유효하지 않은 (Invalid) 숫자<br>
  > 실패한 (Failed) 숫자<br>
  > 몹쓸(?) 숫자<br>
  - 경계 값(Sentinel Value)
  - 반사성(Reflexive)이 없는 `x === x`로 식별되지 않는 값
```javascript
// true
isNaN(NaN);
isNaN("string");

// ES6 Number.isNaN
if(!Number.isNaN) {
	Number.isNaN = function(n) {
		return (
			typeof n === "number" 
			    && window.isNaN(n)
		);
	};
}
Number.isNaN("string"); // false

if(!Number.isNaN) {
	Number.isNaN = function(n) {
		return n !== n;
	}
}
```

<br>

- 무한대<br>
  0으로 나누기(Divide By Zero)
```javascript
// true
1/0 === Infinity;
-1/0 === -Infinity;
Infinity/Infinity === NaN;
```

- 0
  비교연산시, `-0`을 `0`으로 처리한다.<br>
  잠재적인 정보 소실을 방지하기 위해 0의 부호를 보존한 셈이다.
```javascript
// true
0/3 === 0;
0/-3 === -0;

(-0).toString() === "0";
String(-0) === "0";
(-0) + "" === "0";
JSON.stringify(-0) === "0";

+("-0") === -0;
Number("-0") === -0;
JSON.parse("-0") === -0;
```
```javascript
function isNegZero(n) {
	n = Number(n);
	return (n === 0) 
	    && (1/n === -Infinity);
}
```

<br>

- 동등비교: ES6 - Object.is()
```javascript
if(!Object.is) {
	Object.is = function(a, b) {
		// -0 테스트
		if(a === 0 && b === 0) {
			return 1/a === 1/b;
		}
		
		// NaN 테스트
		if(a !== b) {
			return a !== b;
		}
		
		return a === b;
	}
}
```

<br>

# 값 vs 레퍼런스
- 값-복사(Value-Copy)
  - 스칼라 원시 값(Scalar Primitives)
  - `null`, `undefined`, `string`, `number`, `boolean`, `symbol`
- 레퍼런스-복사(Reference-Copy)
  - 합성 값(Compound Values)
  - `object`, object 하위 자식(`array`, `function`)
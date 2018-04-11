# 강제반환
> 강제적 타입변환 (Coercive Type Conversion)<br>
> 타입 강제변환 (Type Coercion)<br>
> 자동 타입변환 (Automatic Type Conversion)<br>

> 암시적이면 강제변환 (Coercion)<br>
> 명시적이면 타입캐스팅 (Type Casting)<br>

> 암시적 강제변환 (Explicit Coercion) - 부수효과 `Hidden Side Effect`를 주의하자!<br>
> 명시적 강제변환 (Implicit Coercion)<br> - 명백한 `Obvious`

> 강제변환을 하면, 문자열, 숫자, 불리언 ... (스칼라 원시 값) 중 하나가 된다. 이때, 객체 (배열, 함수, ...) 같은 합성 값 타입으로 변환될 일은 없다.<br>

```javascript
// + Concatenation
const number = 3;

const stringNumberExplicit = number + ""; // "3"
const stringNumberImplicit = String(number); // "3"
```

<br>

### `toString()`
- `[[Class]]`를 반환한다. - `[object Object]`
 
### `JSON`
- `JSON.stringify()` 
  - JSON 안전 값 (JSON-Safe Value)
  - 안전하지 않은 값: `undefined`, `function`, `symbol`, 환영참조(Circular References)
  - `undefined`, `function`, `symbol`이면 자동 누락
  - 배열에 포함되어 있으면 `null`
  - `toJSON()`: 직렬화 하는 함수, 안전한 JSON을 반환하도록 한다.

```javascript
const o = {
	a: 3,
	b: "3",
	c: [3]
}

JSON.stringify(o, ["a", "b"]); // "{"a":3, "b":"3"}"

// 함수 형태의 대체자 (Replacer)
JSON.stringify(o, function(key, value) {
	if(key !== "a") {
		return value;
	}
}); // "{"b":3, "c":[3]}"


// 스페이스 (Space)
JSON.stringify(o, null, 4);
JSON.stringify(o, null, "--");
```

- 환영참조(Circular References)
  - 메모리 누수(Memory Leak) 발생
  
<br>

### `ToNumber`
```javascript
Number(true); // 1
Number(false); // 0
Number(undefined); // NaN
Number(null); // 0
Number([]); // 0
```
```javascript
// Unary Operator add "+"
+"3"; // 3

- - 3.14; // 3.14

if(!Date.now) {
	Date.now = function() {
		return +new Date();
	}
}
```
> `valueOf()`를 쓸 수 있고, 반환 값이 원시 값이면 그대로 강제반환하되 그렇지 않을 경우 `toString()`을 이용하여 강제 반환한다.<br>

<br>

### `ToBoolean`
```javascript
// falsy
undefined;
null;
false;
+0, 0, -0, NaN;
""
```

<br>

### 틸드 (`~`)
```javascript
// | 연산자는 ToInt32 변환만 수행
0 | -0; // 0
0 | NaN; // 0
0 | Infinity; // 0
0 | -Infinity; // 0

// ~ 연산자는 ToInt32 변환후, NOT 연산 수행 ~x === -(x+1)
~3; // -4
```
```javascript
// 문자열 유무 확인
"abc".indexOf("d") === -1;
~"abc".indexOf("d") === 0;

if(~"abc".indexOf("d")) {
	// 문자열 있음
} else {
	// 문자열 없음
}
```
```javascript
// 비트 잘라내기(truncate) - 32bit 에 한해서 안전
Math.floor(-49.6); // -50
~~-49.6 // -49

/*
 정수로 상위 비트를 잘라낸다.
 x | 0 보다 더 빠르다.
 연산자의 우선순위가 높다.
*/ 
```
<br>

### 문자열 파싱
- `parseInt`, `parseFloat` 
- `parseInt(숫자, 진법)`
- x나 X는 16진법
- o나 O는 8진법
- `toString()`의 리턴 값 사용

<br>

### `valueOf`, `toString`
- 연산을 할때 사용하는 것은 `valueOf`
- String() 할때 사용하는 것은 `toString`

<br>

### 키워드
> 장황함(Verbosity)<br>
> 보일러플레이트(Boilerplate)<br>
> 가환적(Commutative): 연산의 순서를 바꿔도 동일한 것<br> 
> 압축기(Minifier)<br>
> 가드 연산자(Guard Operator)<br>
> 단락 평가(Short Circuiting)<br>
> 느슨한 동등 비교(Loose Equals)<br>
> 엄격한 동등 비교(Strict Equals)<br>
> 추상적 관계 비교(Abstract Relational Comparision)<br>
> 단순어휘(Lexicographic)

<br>

### 추상적 관계 비교(Abstract Relational Comparision)
- toPrimitive 강제변환을 실시하는 것으로 시작한다.

<br>

### 응?!?
```javascript
[] + {} // [object Object]
{} + [] // 0

// 긍정오류 (False Positive)
"0" == false; // true
false == 0; // true
false == ""; // true
false == []; // true
"" == 0; // true
"" == []; // true
0 == []; // true
!![] == true // true

[] == ![]; // true
2 == [2]; // true
"" == [null]; // true
0 == "\n"; // true

// Object - 두 객체가 정확히 똑같은 값에 대한 레퍼런스일 경우 동등

{} == {} //false
({}).toString() == ({}).toString(); // true
```
> 피연산자 중 하나가 true/false일 가능성이 있으면 절대로 == 연산자를 쓰지 말자!<br>
> 피연산자 중 하나가 [], " ", 0이 될 가능성이 있으면 가급적 == 연산자는 쓰지 말자!<br>

> 암시적 강제변환은 변환 과정이 구체적으로 어떻게 일어나는지 명확하게 알고 사용해야 한다, 내가 무슨 코드를 짜고 있고 어떻게 작동할 거란 점은 알고 있어야 한다.<br>
> 다른 개발자들도 쉽게 배우고 이해할 수 있는 코드를 작성하도록 노력하기 바란다.
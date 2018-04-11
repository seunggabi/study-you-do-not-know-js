# `bind`

> `this` 바인딩의 개념을 이해하려면 먼저 호출부, 즉 함수 호출 코드부터 확인하고 `this`가 가리키는 것이 무엇인지 찾아본다.<br>
> 호출 연쇄를 순서대로 잘 따라가면...<br>

<br>

### 기본 바인딩
> 단독 함수 실행 (`Standalone Function Invocation`) - `this`의 기본 (`Default`) 규칙<br>
> 엄격 모드 (`Strict Mode`)에서는 전역 객체가 기본 바인딩 대상에서 제외된다. 그래서 `this`는 `undefined` 된다.<br>
```javascript
function f() {
	"use strict";
	
	console.log(this.n);
}

var n = 1;
f(); // TypeError: this === undefined
```
> 비엄격 모드 (`Non-strict Mode`): 전역 객체만이 기본 바인딩의 유일한 대상<br>

<br>

### 암시적 바인딩
> 호출부에 콘텍스트 객체, 즉 객체의 소유 (`Owning`) / 포함 (`Containing`) 여부를 확인<br>
> 함수전달하여 객체로 실행
```javascript
function f() {
	console.log(this.n);
}
function go(fn) {
	fn();
}

var n = 1;
var o = {
	n: 3,
	f: f
}

o.f(); // 3 - 암시적 바인딩

var ff = o.f;
ff(); // 1 - 함수 래퍼런스/별명

go(f); // 1 - 함수 래퍼런스/별명
```

<br>

### 명시적 바인딩
> 유틸리티(`[[Prototype]]`)을 사용해서~<br>
> `call()`, `apply()`<br>

<br>

- 하드 바인딩: 재사용 가능한 헬퍼(Reusable Helper) 함수
```javascript
function bind(fn, obj) {
	return function() {
		return fn.apply(obj, arguments);
	}
}
```

<br>

- `new` 바인딩
> 클래스 지향(`Class-Oriented`) / 생성자(`Constructor`)<br>
> 생성자: new 연산자가 있을 때 호출되는 일반함수이다.<br>

1. 새 객체가 만들어진다.
2. 새 객체와 `[[Prototype]]`이 연결된다.
3. 새 객체의 `this`로 바인딩된다.
4. 이 함수가 다른 객체를 반환하지 않는한, new로 새로운 객체를 반환한다.

```javascript
// 함수 인자를 전부 또는 일부만 미리 세팅
// bind() 함수는 최천 this 바인딩 이후 전달된 인자를 원 함수(`Underlying Function`)의 기본 인자로 고정한다.
// 부분적용(`Partial Application`) / 커링(`Currying`)

function f(p1, p2) {
	this.value = p1 + p2;
}
var ff = f.bind(null, "p2");
var fff = new ff("p1");

console.log(fff.value); // p1p2
```

<br>

- `this` 확정 규칙
1. `new` 
2. `call`, `apply`, `bind`
3. 암시적 바인딩
4. 그외 기본값

<br>

### 바인딩 예외
> `call`, `apply`, `bind`에 `null` 이나 `undefined`를 넘기면, 기본 바인드 규칙 적용
```javascript
function f() {
	console.log(this.n);
}

var n = 2;
f.call(null); // 2
```
<br>

### 빈 객체
```javascript
Object.create(null); // Object.prototype으로 위임하지 않음
{}; // Object.prototype으로 위임함
```

### 화살표 함수 (`Arrow Function`)
```javascript
() => {} // this 알아서 바인딩됨
```
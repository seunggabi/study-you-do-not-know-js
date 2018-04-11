# 호이스팅

> 선언문이 대입문 보다 먼저다.

```javascript
console.log(a); // undefined
var a = 2;
console.log(a); // 2


// 아래처럼 동작한다.
var a;
console.log(a); // undefined
a = 2;
console.log(a); // 2
```
<br>

### 함수 표현식이 이름을 가져도 그 이름 확인자는 해당 스코프에서 찾을 수 없다.
```javascript
f(); // TypeError
v(); // ReferenceError

var f = function v() {
	// ...
}


// 아래처럼 동작한다.
var f;
f(); // TypeError
v(); // ReferenceError

f = function () {
	var v = this;
}
```

### 함수가 먼저다.
- 이것은 `선언이 대입 우선이다` 를 통해서 생각하면 쉽다.
```javascript
f(); // function2
var f;

function f() {
	console.log("function");
}

f = function() {
	console.log("var");
}

function f() {
	console.log("function2")
}
```
> 많은 중복 변수 선언문이 사실상 무시됐지만 중복 함수 선언문은 앞선다. (`override`)

> 블록 내 함수 선언은 지양하는 것이 가장 좋다.

> 선언문 자체는 옮겨지지만, 모든 대입문은 끌어올려 지지 않는다.<br>
> 중복선언을 조심하자!
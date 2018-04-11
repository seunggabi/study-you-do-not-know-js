# 스코프 클로저

> 콜로저는 함수를 렉시컬 스코프 밖에서 호출해도 함수는 자신의 렉시컬 스코프를 기억하고 접근할 수 있는 특성을 의미

> 거의 신화적인 부분인 클로저 ...

> 완전히 이해하고 있었지만, ... 무엇인지도 몰랐던 적이 있었다.

> 처음으로 `모듈 패턴`과 비슷한 것

- 클로저는 렉시컬 스코프에 의존해 코드를 작성한 결과로 그냥 발생한다.
- 모든 코드에서 클로저는 생성되고 사용된다.

### 클로저란?
- 렉시컬 스코프 검색 규칙을 통해 바깥 스코프의 변수에 접근할 수 있다.
- 여전히 `사용중`이므로 해제되지 않는다.
- 렉시컬 스코프 클로저를 가지고 나중에 참조할 수 있도록 스코프를 살려둔다.
<br><br>
- 호출된 함수가 원래 선언된 렉시컬 스코프에 계속해서 접근할 수 있도록 허용한다.
- 어떤 방식이든 함수를 값으로 넘겨 다른 위치에서 호출하는 행위
- 콜백함수를 넘기면 클로저를 사용할 준비가 된 것이다.
<br><br>
- `IIFE`: 스코프를 생성하지만, 함수는 자신의 렉시컬 스코프 밖에서 실행된 것이 아니다.
  
```javascript
for (var i=1; i<=5; i++) {
	let j = i;
	setTimeout(function timer() {
		console.log(j);
	}, j*1000);
}
```
```javascript
for (let i=1; i<=5; i++) {
	setTimeout(function timer() {
		console.log(i);
	}, i*1000);
}
```

<br>

### 모듈
```javascript
// 모듈노출 Revealing Module
function Module() {
	var v = "var";
	
	function _get() {
		return v;
	}
	
	function _set(value) {
		v = value;
	}
	
	var publicAPI = {
		get: _get,
		set: _set
	};
	return publicAPI;
}
```
- 인스턴스를 생성하려면 반드시 호출해야 한다.
  - 최외곽 함수가 실행되지 않으면 내부 스코프와 클로저는 생성되지 않는다.
- 모듈의 공개 API라고 한다.

<br>

### 싱글톤 (`Singleton`)만 생성하는 모듈: 모듈 + IIFE
```javascript
var singleton = (function Module() {
	var v = "var";
	
	function _get() {
		return v;
	}
	
	function _set(value) {
		v = value;
	}
	
	var publicAPI = {
		get: _get,
		set: _set
	};
	return publicAPI;
})();

singleton.get();
singleton.set();
```

<br>

### 현재의 모듈
```javascript
var module = (function Manager() {
	var modules = {};
	
	function _define(name, deps, impl) {
		for(var i=0; i<deps.length; i++) {
			deps[i] = modules[deps[i]];
		}
		modules[name] = impl.apply(impl, deps);
	}
	
	function _get(name) {
		return modules[name];
	}
	
	var publicAPI = {
		define: _define,
		get: _get
	}
	return publicAPI;
})
```

<br>

### 미래의 모듈
> ES6 모듈은 inline 형식을 지원하지 않고, 반드시 개별 파일(모듈당 파일 하나)에 정의되어야 한다.<br>
> 모듈을 불러올 때 모듈 로더는 동기적으로 모듈 파일을 불러온다.
```javascript
// f.js
function f() {
	console.log("f");
}

// index.js
module f from "f";

f.f();
```
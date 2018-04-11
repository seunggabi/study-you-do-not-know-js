# 네이티브
> 특정 환경(브라우저 등의 클라이언트 프로그램)에 종속되지 않은, ECMAScript 명세의 내장 객체<br>
> 
> 내장함수들...<br>
> String()<br>
> Number()<br>
> Boolean()<br>
> Array()<br>
> Object()<br>
> Function()<br>
> RegExp()<br>
> Date()<br>
> Error()<br>
> Symbol()<br>
> ...

<br>

네이티브는 생성자 처럼 사용할 수 있지만 실제로 생성되는 결과물은 다를 수 있다.
(브라우저마다 다르다. 어떻게 객체를 직렬화하여 보여주는 편이 좋을지는 브라우저 개발자가 임의로 정했기 때문이다.) 

### 내부 [[Class]]
> 전통적인 클래스 지향 Class-Oriented 개념의 클래스라기 보다는,<br>
> 내부 분류법 Classification의 일부로 보자!

```javascript
Object.prototype.toString.call([1,2,3]); // [object Array]
Object.prototype.toString.call(/regex-literal/i); // [object RegExp]
Object.prototype.toString.call(null); // [object Null]
Object.prototype.toString.call(undefined); // [object Undefined]
```

<br>

> `string`, `number`, `boolean` 같은 단순 원시 값은<br>
> 박싱(`Boxing`) 과정을 거친다.<br>
> 객체 래퍼로 자동 박싱된다.<br>
```javascript
Object.prototype.toString.call("string"); // [object String]
Object.prototype.toString.call(3); // [object Number]
Object.prototype.toString.call(true); // [object Boolean]
```

### 래퍼로 박싱
> 원시 값엔 프로퍼티나 메서드가 없으므로<br>
> `length`, `toString()`을 사용하려면 객체 래퍼로 감싸줘야 한다.

> 개발자가 직접 객체 형태로 선 최적화(Pre-Optimize)하면 프로그램이 더 느려질 수 있다. 따라서, 엔진이 알아서 박싱하게 하는 것이 낫다.

> 수동으로 박싱이 필요한 경우 `Object()`를 이용하자! 이때 `new` 키워드는 없다.
```javascript
var b = new Boolean(false);

if(!b) {
	console.log("call?");
}

// 객체는 `truthy`라서 call?이 찍히지 않는다.
```

<br>

### 언박싱 
```javascript
var s = new String("string");
var n = new Number(3);
var b = new Boolean(false);

console.log(s.valueOf()); // string
console.log(n.valueOf()); // 3
console.log(b.valueOf()); // false

// 암시적인 언박싱 발생
```

<br>

### 네이티브, 나는 생성자다
- `Array()` - `new`를 붙이지 않아도 된다.
  - 인자 1개인 경우, 배열의 크기를 미리 정하는 `Presize` 기능 // new Array(3) - length === 3
  - `var a = Array.apply(null, {length:3});`

<br>

- `Object`, `Function`, `RegExp`
> 선택사항이다. 어떤 분명한 의도가 아니라면 사용하지 않는 편이 좋다.<br>
> `new Object()` 사용할 일이 없다.<br>
> `new Function()` 함수 인자/내용 동적으로 정의할 때 필요하지만, 그럴일이 거의 없다.<br>
> `new RegExp()` 정규 표현식은 리터럴 형식(`/^a*b+/g`)으로 사용하는 것을 권장한다.
```javascript
// new RegExp("패턴", "플래그");
new RegExp("\\b(?:" + name + ")+\\b", "ig");
```

<br>

- `Date`, `Error`
> `new Date()`<br>
> `Date.now()`<br>
```javascript
if(!Date.now) {
	Date.now = function() {
		return (new Date()).getTime();
	}
}
```
```javascript
function foo(x) {
	if(!x) {
		throw new Error("!!!");
	}
	// ...
	
	// EvalError();
	// RangeError();
	// ReferenceError();
	// SyntaxError();
	// TypeError();
	// URIError();
}
```

<br>

- `Symbol`
> 심벌은 객체가 아니다. 단순 스칼라 원시 값이다.
```javascript
obj[Symbol.iterator] = function() {};

var sym = Symbol("symbol");
console.log(sym); // Symbol(symbol)
typeof sym // symbol

var o = {};
o[sym] = "sym";
Object.getOwnPropertySymbols(o); // Symbol(symbol)
```
<br>

- `prototype` 
> 디폴트 값이다.<br>
> ES6 부터는 `vals = vals || 디폴트 값` 구문은 더 이상 필요 없다.



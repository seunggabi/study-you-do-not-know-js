# 타입

### 내장타입 
`null`, `undefined`, `boolean`, `number`, `string`, `object`, `symbol`<br>
- `symbol` (ES6 부터 추가)
- `object`를 제외한 타입을 원시타입(Primitive)라고 한다.
- 값은 타입을 가지지만, 변수는 ~~타입~~을 갖지 않는다.
- `typeof`의 반환값은 항상 문자열
 
```javascript
//false
typeof null === "null" // "object"

//true
typeof null === "object"
typeof undefined === "undefined"
typeof true === "boolean"
typeof false === "boolean"
typeof 3 === "number"
typeof "3" === "string"
typeof {number: 3} === "object"
typeof Symbol() === "symbol" // ES6 부터 추가
```
<br>

### null?
```javascript
var n = null;
var isNull = (!n && typeof n === "object");
```

<br>

### function?
```javascript
var f = function (a, b){};

//true
var isFunction = (typeof f === "function"); // object 하위타입 function
var countOfArgs = (f.length === 2);
```

<br>


### array?
```javascript
var a = [1, 2, 3];

//true
(typeof a === "object"); // object 하위타입 object - length 자동관리
var countOfElments = (a.length === 3);
```

<br>


### undefined?
- 값이 없는, undefined
- 선언되지 않은, undeclared
- 값이 `undefined`로 같은 것 같지만, 전혀 다른 의미이다!

```javascript
// 안전 가드(safety guard)
// 값이 없는, 선언되지 않은 2가지 경우를 모두 대응할 수 있다.

if(typeof DEBUG !== "undefined") {
	console.log("start debugging!");
}
```
```javascript
// 전역객체 사용 window
// (주의사항) 다중 자바스크립트 환경 nodejs에서는, 
//           window를 global로 사용하므로 동작하지 않는다.

if(window.DEBUG) {
	console.log("start debugging!");
}
```

<br>

### 선언되지 않은, 대처 방법
```javascript
// 1. 변수에 할당
var util = (typeof utilFunction !== "undefined") ? utilFunction : function (){};
```
```javascript
// 2. 의존성 주입(Dependency Injection)
function setting(utilFunction) {
    var util = utilFunction || function (){};
}
```
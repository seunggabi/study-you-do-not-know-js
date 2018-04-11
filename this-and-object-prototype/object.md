# 객체

- 키워드
  - 원시리터럴(`Primitive Literal`)
  - 불변값(`Immutable Value`)
  - 자동 강제변환(`Coerce`)
  - 구현 의존적(`Implementation-Dependent`)
  - 객체 컨테이너(`Object Container`)
  - 얕은 복사(`Shallow Copy`)
  - 깊은 복사(`Deep Copy`)
  - 원 객체(`Original Object`)
  - 환형 참조(`Circular Reference`)
  - 환형 순회(`Circular Traversal`)


### 선언
```javascript
// 선언적(`Declarative`/`Literal`)
var o = {
	key: value,
	// ...
};

// 생성자
var o = new Object();
o.key = value;
```
> 단순한 원시타입(`Simple Primitive`): `string`, `number`, `boolean`, `null`, `undefined` 는 객체가 아니다.<br>
> 언어 자체의 버그(`typeof null === "object"`)로 인한 오해<br>
> 복합 원시타입(`Complex Primitives`) 독특한 객체 하위 타입(`Sub Type`)이 있다. - `function`, `Array`, ...<br>

<br>

### 프로퍼티 
```javascript
var o = {
	n: 1
};

/*
 프로퍼티 접근(Property Access)
 식별자 호환(Identifier-Compatible) 해야함.
*/

o.n;

/*
 키 접근(Key Access)
 UTF-8/유니코드 호환 문자열이라면 가능.
*/

o["n"];
```
> 문자열 이외의 다른 원시 값을 쓰면 우선 문자열로 변환된다.<br>
> 배열 인덱스로 사용하는 숫자도 마찬가지이므로 공연히 객체와 배열 사이에 숫자를 써서 헷갈리는 코드를 만들지 않도록 하자.<br>

<br>

```javascript
// [ES6~] 계산된 프로퍼티명
var prefix = "prefix_";

var o = {
	[prefix + "n"]: 1
};
```

<br>

> 객체에 참조된 함수를 메서드라 칭할 수 없다.<br>
> 개별 레퍼런스일 뿐 소유한 함수라는 의미가 아니기 때문이다.<br>

> [ES6~] `super` 레퍼런스가 더해져서 `class`와 함께 사용할 수 있다. 정적 바인딩 방식으로 동작하므로 `함수`보다는 `메서드`에 가까운 `super` 바인딩 함수라는 사상을 강조하지만 의미상, 체계상의 미묘한 차이가 있다.

<br>

- 배열의 프로퍼티
  - 숫자와 유사하면 배열의 길이가 달라진다.
  - 숫자와 유사하지 않으면 배열의 길이는 변하지 않는다.
  
<br>

### 복사
```javascript
/*
 [Copy]
 JSON 안전한 객체여야 한다.(환형 참조가 없는)
 참조된 함수 프로퍼티는 복사되지 않는다.
*/

var newObject = JSON.parse(JSON.stringify(originObject));
```

<br>

```javascript
// [ES6~] Object.assign(TargetObject, SourceObjects, ...)
// 열거 가능한 것(Enumerable)과 보유키(Owned Keys)를 순회하면서 할당문으로 복사한다. 
var newObject = Object.assign({}, originObject);
```
> `Object.assign()`은 순수하게 `= 할당`에 의해서만 복사하므로 `writable` 같은 특수한 프로퍼티의 속성은 타깃 객체에 복사되지 않는다.

<br>

### 프로퍼티 서술자
- 쓰기가능(`writable`)
```javascript
var o = {};
Object.defineProperty(o, "n", {
	value: 1,
	writable: false, // 쓰기금지
	configurable: true,
	enumerable: true
});

// 비엄격모드는 조용히 실패 / 엄격모드에서는 에러
o.n = 2; // TypeError
```

<br>

- 설정가능(`configurable`)
  - `defineProperty`로 프로퍼티 서술자를 변경
  - `configurable`이 `false`가 되면 돌아올 수 없는 강을 건너게 되어 절대로 복구되지 않으니 유의
  - `delete` 프로퍼티 삭제도 금지

<br>

> 봉인(`Seal`) 또는 동결(`Freeze`)해야 하는 상황이라면 객체 값이 변경되어도 문제가 없는 견고한 프로그램을 설계할 다른 방법은 없는지 일반적인 디자인 패턴 관점에서 재고 해야한다.
- 불변성
  - 객체상수: writable: `false`,  configurable: `false`
  - 확장금지: `Object.preventExtensions()`
  - 봉인: `Object.seal()`
    - `Object.preventExtensions()`
    - configurable: `false`
  - 동결: `Object.freeze()`
    - `Object.seal()`
    - writable: `false`
    - 참조하는 모든 객체를 재귀 순회하면서 `Object.freeze()`를 적용하여 깊숙이 동결(`Deep Freeze`) 한다.  

<br>

- 열거기능(`enumerable`)
> 객체 프로퍼티 순회 리스트에 포함
> `for ... in` 연산자는 어떤 프로퍼티가 해당 객체에 존재하는지 아니면 이 객체의 `[[Prototype]]` 연쇄를 따라갔을 때 상위 단계에 존재하는지 확인한다.<br>
> `hasOwnProperty()`는 단지 프로퍼티가 객체에 있는지만 확인하고 `[[Prototype]]` 연쇄는 찾지 않는다.
```javascript
var o = {}

Object.defineProperty(o, "a", {
	enumerable: true,
	value: 1
});

Object.defineProperty(o, "b", {
	enumerable: false,
	value: 2
});

o.propertyIsEnumerable("a"); // true
o.propertyIsEnumerable("b"); // false

Object.keys(o); // ["a"]
Object.getOwnPropertyNames(o); // ["a", "b"]
```

<br>

### `[[Get]]`
> 주어진 프로퍼티 값을 어떻게 해도 찾을 수 없으면 `[[Get]]` 연산은 `undefined`를 반환한다.<br>
> 렉시컬 스코프 내에 없는 변수를 참조하면 객체 프로퍼티처럼 `undefined`가 반환되지 않고 `ReferenceError`가 발생한다.

### `[[Put]]`
1. 프로퍼티가 접근서술자(`Accessor Descriptor`)인가?
2. 프로퍼티가 `writable:false`인가? 실패
3. 이외에는 프로퍼티에 해당 값을 세팅한다.

```javascript
var o = {
	get n() {
		return this._n_;
	},
	
	set n(value) {
		this._n_ = value;
	}
}
```

<br>

### 순회
- `ES5~`
  - `forEach()`: 콜백 반환 값 무시
  - `every()`: 콜백 반환 값이 `false` 이면 중단
  - `some()`: 콜백 반환 값이 `true` 이면 중단

<br>
  
- `ES6~`: `for ... of`
  - 순회할 원소의 순회자 객체 (`Iteration Object`)
  - 순회자 객체의 `next()` 메서드를 호출하여 연속적으로 반환 값을 순회한다.
  - 배열은 `@@iterator` 가 내장되어 있다.
```javascript
var a = [1, 2, 3];
var i = a[Symbol.iterator]();

i.next(); // { value:1, done:false }
i.next(); // { value:2, done:false }
i.next(); // { value:3, done:false }
i.next(); // { done:true }
```

```javascript
var o = {
	a: 1,
	b: 2
};

// iterator을 직접 선언할 수 있다.
Object.defineProperty(o, Symbol.iterator, {
	enumerable: false,
	writable: false,
	configurable: true,
	value: function() {
		var self = this;
		var idx = 0;
		var ks = Object.keys(self);
		return {
			next: function() {
				return {
					value: self[ks[idx++]],
					done: (idx > ks.length)
				};
			}
		};
	}
});

var i = o[Symbol.iterator]();
i.next(); // { value:1, done:false }
i.next(); // { value:2, done:false }
i.next(); // { done:true }
```
> ES6의 `for ... of` 와 `커스텀순회자`는 사용자 정의 객체를 조작하는 데 아주 탁월한 새로운 구문 도구(`Syntactic Tool`)다.
```javascript
var randoms = {
	[Symbol.iterator]: function() {
		return {
			next: function() {
				return { value: Math.random() };
			}
		};
	}
};

var randoms_pool = [];
for (var n of randoms) {
	randoms_pool.push(n);
	
	if(randoms_pool.length === 100) break;
}
```
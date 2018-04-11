# 프로토타입 `[[Prototype]]`

### 프로토타입 연쇄(`Prototype Chain`)
  - 객체의 연쇄를 전부 샅샅이 뒤진다.
  - 발견되지 않으면 `undefined`
  - 동시에 발견될 때, 가려짐(`Shadowing`) - 상위 수준의 프로퍼티가 읽기전용인 경우(`writable:false`), 발생X
  
<br>
  
### 클래스
> 일종의 클래스 같은 독특한 작동은 모든 함수가 기본으로 프로토타입이라는 공용(`Public`)/열거불가(`Nonenumerable`) 프로퍼티를 가진다는 이상한 특성에 기인한다.
```javascript
function O() {
	// ...
}

O.prototype; // {}

// 삼수는 결코 생성자가 아니지만 new를 사용하여 호출할 때에만 `생성자호출`이다.
var o = new O();
Object.getPrototypeOf(o) === O.prototype; // true
```
> 자바스크립트에서 객체들은 서로 완전히 떨어져 분리되는 것이 아니라 끈끈하게 연결된다. - 프로토타입 상속(`Prototypal Inheritance`)

> `constructor`은 매우 불안정하고 신뢰할 수 없는 레퍼런스이므로 될 수 있는 대로 코드에서 직접 사용하지 않는 게 상책이다.
<br>

### 프로토타입 상속
```javascript
function A() {
	// ...
}
function B() {
	// ...
}

// A 프로토타입과 연결된 새로운 B 프로토타입을 만들라는 뜻
B.prototype = Object.create(A.prototype);

// A 생성자 호출로인한, 사이드이팩트 발생
B.prototype = new A();

// A 프로토타입과 연결된 새로운 B 프로토타입을 만들라는 뜻, 사이드이팩트 발생하지 않음.
B.prototype = Object.create(A.prototype);
```
> `Object.create()`를 쓰도록 하자!
```javascript
// ~ ES5: 더 짧고 가독성이 좋다.
B.prototype = Object.create(A.prototype);

// ES6 ~
Object.setPrototypeOf(B.prototype, A.prototype);
```

<br>

### 클래스 관계
- 상속계통 === 위임링크(`Inheritance Ancestry`) 
- 인트로스펙션(`Introspection`) === 리플렉션(`Reflection`): 위임링크을 살펴보는 것
```javascript
function A() {
	// ... 
}
var a = new A();

a instanceof A; // true
A.prototype.isPrototypeOf(a); // true
Object.getPrototypeOf(a) === A.prototype; // true
a.__proto__ === A.prototype; // true

Object.defineProperty(Object.prototype, "__proto___", {
	get: function() {
		return Object.getPrototypeOf(this);
	},
	set: function(o) {
		// ES6 부터는 setPrototypeOf 사용
		Object.setPrototypeOf(this, o);
		return o;
	}
})
```

<br>

### 객체 링크
- `Object.create(null)`
  - `[[Prototype]]` 링크가 빈(null) 객체를 생성하므로 이젠 위임할 곳이 전혀없다.
  - `instanceof` 연산의 결과는 `false`
  - 일차원적인 데이터 저장소로 제격이므로 순수하게 프로퍼티에 데이터를 저장하는 용도로 사용
  - 딕셔너리 (`Dictionary`)라 한다

```javascript
// Object.create() 폴리필
if(!Object.create) {
	Object.create = function(o) {
		function F() {}
		F.prototype = o;
		return new F();
	}
}
```

- `ES6`에는 프랏시라는 고급 기능이 추가되어 메서드가 발견되지 않으면, `Method not found` 유형의 로직을 제공한다.
- 위임 디자인 패턴 (`Delegation Design Pattern`): 구현 상세를 겉으로 노출하지 않고 내부에 감추는 식으로 위임함.

<br>

### 키워드
- 차등상속(`Differential Inheritance`): 어떤 객체의 작동을 더 일반적인 객체와 비교했을 때 어느 부분이 다른지 기술하는 아이디어
- 멘탈모델(`Mental Model`): 개발자의 머릿속에서만 그려진 일련의 로직
- 단터(`Dunder`): `__proto__`를 던더프로토라고 한다.

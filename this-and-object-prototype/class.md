# 클래스와 객체의 혼합

- 키워드
  - 객체지향프로그래밍 (`OOP`, `Object-Oriented Programming`)
  - 인스턴스화 (`Instantiation`)
  - 상속 (`Inheritance`) === 확장 (`Extend`)
  - 다형성 (`Polymorphism`)
  - 클래스지향 (`Class Orientation`)
  - 캡슐화 (`Capsulation`)
  - 자료구조 (`Data Structure`)
  - 분류 (`Classify`)
  - 기반정의 (`Base Definition`)
  - 세분화 (`Specialize`)
  - 간편구문 (`Syntactic Sugar`)
  - 아키텍트 (`Architect`)
  - 아키텍처 청사진 (`Architectural Blueprint`)
  - 패턴
    - 순회자 (`Iterator`)
    - 관찰자 (`Observer`)
    - 팩토리 (`Factory`)
    - 싱글턴 (`Singleton`)

<br>

### 생성자
```javascript
class Hello {
	world() {
		console.log("Hello world!");
	}
}

var h = new Hello();
h.world();
```

<br>

### 상속
> 부모클래스 (`Parent Class`), 자식클래스 (`Child Class`)<br>
> 새로운 방식으로 오버라이드할 수 있다. 인스턴스해야 한다.

> `[[Prototype]]` 체계의 작동 방식

> 다중상속 (`Multiple Inheritance`): 마름모 문제(`Diamond Problem`), 자바스크립트는 지원하지 않는다.

<br>

### 믹스인
```javascript
function mixin(source, target) {
	for(var key in source) {
		if(!(key in target)) {
			target[key] = source[key];
		}
	}
	
	return target;
}
```

<br>

### 다형성
- 명시적 의사다형성 (`Explicit Pseudopolymorphism`): `Object.fn.call(this)`
  > 명시적 의사다형성은 아무리 봐도 장점보다는 비용이 훨씬 더 많이 들기 때문에 가능한 한 쓰지 않는 게 좋다.

<br> 

### 정리
- 근본적으로 '꼼수'를 쓰면 대개 얻는 것보다 잃을 것이 (성능도 떨어진다!) 더 많다.
- 자바스크립트에서 클래스의 의미는 다른 언어와 다르다. (클래스는 복사를 의미한다.)
- `부모 -> 자식` 방향으로 복사
- 믹스인은 복사기능이 아니라 공유된 레퍼런스만 생성!
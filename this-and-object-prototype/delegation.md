# 작동위임(`Behavior Delegation`) === 위임 지향 디자인(`Object Linked to Other Object`)

### 의미
- 프로토타이핑 체이닝을 사용해서 `객체를 다른 객체와 연결하는 것`
- 프로퍼티/메서드 래퍼런스가 객체에 없으면 다른 객체(`Task`)로 수색작업을 위임하는 것을 의미한다.\
- 형제(`Sibling`) or 동료(`Peer`) 객체와의 연결

<br> 

### 상호위임(`Multual Delegation`) X
- 사이클(`Cycle`)로 서로가 서로를 위임하는 것은 허용하지 않는다.

<br>

```javascript
// Object Oriented
function Foo(who) {
    this.me = who;
}
Foo.prototype.identify = function() {
    return "identify: " + this.me;
}

function Bar(who) {
    Foo.call(this, who);
}
Bar.prototype = Object.create(Foo.prototype);

Bar.prototype.speak = function() {
    console.log("Hello " + this.identify());
}

var b1 = new Bar("b1");
var b2 = new Bar("b2");

b1.speak();
b2.speak();



// Object Linked Other Object
Foo = {
    init: function(who) {
        this.me = who;
    },
    identify: function() {
        return "identify: " + this.me;
    }
};

Bar = Object.create(Foo);
Bar.speak = function() {
    console.log("Hello " + this.identify());
}

var b1 = Object.create(Bar);
b1.init("b1");
var b2 = Object.create(Bar);
b2.init("b2");

b1.speak();
b2.speak();
```

<br>

### 위젯클래스
- 기본 기능을 완전히 갈아치운다기보단 버튼에 해당하는 작동을 덧붙여 기본 기능을 증강 (`Augment`) 한다.
- 단독으로 존재하는 객체 (`Standalone Object`)
- 생성 및 초기화 과정을 굳이 한곳에 몰아넣고 실행하지 않아도 되니, 관심사 분리의 원칙 (`Principle of Separation of Concerns`)를 잘 반영한 패턴

<br>

### 인트로스펙션
> 객체 유형을 거꾸로 유추하는 타입 인트로스펙션 (`Type Introspection`)
- 덕타이핑(`Duck Typing`)
  - ES6의 `Promise`: `then` 함수를 가졌는지?, `Thenable` 한지?
```javascript
if(a.something) {
	a.something();
}
```





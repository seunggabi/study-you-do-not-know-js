# 문법

> 문법 (Grammar)<br><br>
> 구문 (Syntax)<br>
> 표현식 (Expression) / 문장 (Sentence) / 어구 (Phrase) / 구두점 (Punctuation) / 접속사 (Conjunction)<br>
> 선언문 (Declaration Statement)<br>
> 할당 표현식 (Assignment Expression)<br>
> 단독 문 (Standalone Statement)<br>
> 연쇄 할당문 (Chained Assignment)<br>
> 완료 값 (Completion Value)<br>
> 단락(Short Circuited): 가능한 한 빨리 지름길을 택한다.<br>

> 분해 할당 (Destructuring Assignments)<br>
```javascript
var {a, b} = ...;
// {a, b}는 {a: a, b: b}를 간결하게 쓴 형태 간편 구문 (Sugar Syntax)
```

<br>

> `{ }` 은 전적으로 사용 콘텍스트에 따라 의미가 결정

> else if는 정확한 문법 규칙이 아니다.
```javascript
// if ~ else if

if(...) {
	
} else {
	if(...) {
		
	}
	else {
		
	}
}
```

<br>

### 연산자 우선순위 (Operator Precedence)
```javascript
false && true || true // true

// &&는 언제나 || 보다 먼저 평가되고, 좌측 -> 우측
// ||는 (a ? b : c) 보다 먼저 평가되고, 좌측 <- 우측
```

### 결합성 (Grouping)
> 좌측 결합성(Left Associative)<br>
> 우측 결합성(Right Associative): `(a ? b : c)`, `=`<br>
```javascript
a = b = c = 42;
a = (b = (c = 42));
```
> 우선순위와 결합성을 적절히 활용하여 짧고 깔끔한 코드를 작성하되, 혼동을 줄이고 분명히 밝혀야 할 부분은 `( )`로 예쁘게 감싸 주기 바란다.

<br>

### ASI (Automatic Semicolon Insertion) - 자동 세미콜론 삽입
> 대부분 세미콜론은 선택사항이다.(`for( ; ; )`처럼 필수 세미콜론도 있다.)<br>
> 타이핑을 아끼면서 '아름다운 코드'를 작성하겠다는 발상 자체가 터무니 없다.<br>
> Python - 유효공백 문자 (Significant Whitespace)<br>
- `break`, `continue`, `return`, `yield(ES6~)` 에서 요긴하게 쓰인다.

<br>

### Error
> `TypeError`, `ReferenceError`, `SyntaxError`<br>
> `Early Error`: 코드가 실행도 되기 전에 발생하므로, `try ... catch`로 잡을 수 없으며, 그냥 프로그램 파싱/컴파일이 실패한다.<br>

> 올바르지 안은 정규식은 에러를 던진다.<br>
> 할당 대상은 반드시 식졀자다.<br>
> 엄격 모드에서 함수 인자명은 중복될 수 없다.<br>

<br>

### 임시 데드 존(TDZ, Temporal Dead Zone) ES6~
- 아직 초기화를 하지 않아 변수를 참조할 수 없는 코드 영역

<br>

### try ... finally
- finally 절에서 예외가 던져지면, 이전의 실행 결과는 모두 무시한다.
- finally 의 return 은 try 나 catch 절의 return 을 덮어쓰는 능력이 있다
 
<br>

### yield
- 중간 (intermediate) return 문 같은 발생자 (Generator)


### switch
- case 표현식의 결과를 엄격하게 매치한다.
- true/false로 떨어지게 `case !!(변수):`로 작성한다.
# 스코프란 무엇인가?

> 프로그래밍 언어의 기본 패러다임 중 하나는 변수에 값을 저장하고 저장된 갑사을 가져다 쓰고 수정하는 것.
 
> 변수는 어디에 살아있는가?, 변수는 어디에 저장되는가?

> `스코프 (Scope)`: 특정 장소에 변수를 저장하고 나중에 그 변수를 찾는 데는 잘 정의된 규칙이 필요하다는 점이다.

<br>

### 컴파일러 이론
> 자바스크립트는 컴파일러 언어이다. `컴파일레이션 (Compileation)` 이라고 한다.
- `토크나이징 (Tokenizing)` / `렉싱 (Lexing)`<br>
  의미 있는 조각으로 만드는 과정
  
- `파싱 (Parsing)`
  `AST (Abstract Syntax Tree)`: 추상구문트리
  
- 코드 생성(Code-Generation)<br>
  자바스크립트 엔진이 기존 컴파일러와 다른점: 컴파일레이션을 미리 수행하지 않아서 최적화할 시간이 많지 않다는 점<br>
  `레이지컴파일 (Lazy Compile)`, `핫리컴파일 (Hot Recompile) JITs` 사용<br>
  실행전에 컴파일되어야 한다는 것이다.<br>
  
<br>

### 스코프 이해하기
> 엔진: 실행을 책임진다.<br>
> 컴파일러: 파싱과 코드생성<br>
> 스코프: 모든 확인자(변수) 검색목록을 작성하고 유지, 엄격한 규칙으로 실행 코드에서 확인자의 적용 방식을 정함.<br>

> 컴파일러가 다음 의사코드(Pseudo-Code)로~, 중첩 스코프 부분
 
<br>

### 컴파일러체
> 선언된 적이 있는지 스코프에서 검<br>
> `LHS (Left-Hand Side)`: 대입할 대상<br>
> `RHS(Right-Hand Side)`: 대입한 값<br>
> `RHS (Right-Hand Side)` Retrieve (가져오라) his/her (그의/그녀의) source (소스) - 가서 값을 가져오라.<br>

<br>

### 중첩 스코프
> 중첩 (Nested), 글로벌 스코프<br>
```javascript
// 건물
// ooooo <- 글로벌 스코프
// ooooo
// ooooo
// ooooo <- ... 렉시컬 스코프
// ooooo
// ooooo 
// ooo.o <- 현재 실행중인 스코프
```

<br>

### 오류
변수가 선언되지 않았을때?<br>
`LHS`: `ReferenceError`
`RHS`: `TypeError`

> ES5 부터 지원하는 `Strict` Mode는 `Normal`/`Relaxed`/`Lazy` Mode와는 다르게 작동한다.

# 지금과 나중
> 지금(`Now`)와 나중(`Later`) 사이의 간극(`Gap`)<br>
> 지금에 해당하는 부분 그리고 나중에 해당하는 부분 사이의 관계가 바로 비동기의 핵심이다.<br>
> 일급 프로그래밍 언어(`First-Class Programming Language`)<br>

<br>

### 비동기 
- 단위는 함수(`Function`)
- 지금 당장 끝낼 수 없는 작업은 비동기적으로 처리되므로, 중단(`Blocking`)하지 않는다.
- 지금부터 나중까지 기다리는 가장 간단한 방법은 콜백(`Callback Function`)이다.
- 프로그램에 비동기성을 부여

> 많은 프로그램에서 I/O 부분이 가장 느리고 중단이 잦다.<br>
> I/O를 백그라운드에서 비동기적으로 처리해야 성능상 유리하다.<br>

<br>

### 이벤트 루프
- 스레드(`Thread`)는 공통
- 여러 프로그램 덩이를 시간에 따라 매 순간 한번씩 엔진을 실행시키는 장치

```javascript
// Queue
var eventLoop = [];
var event;

while(true) {
    if(eventLoop.length > 0) {
        event = eventLoop.shift();
        
        try {
            event();
        } catch(err) {
            reportError(err);
        }
    }
}
```
- 매 순회(`Iteration`)을 틱(`Tick`)이라고 한다.
- 적재된 이벤트를 꺼내어 실행
- `setTimeout()`은 콜백을 이벤트 루프 큐에 넣지 않는다.
  - 타이머가 끝나면 환경이 콜백을 이벤트 루프에 삽입한 뒤 틱에서 콜백을 꺼내어 실행.
  
<br>

### 병렬 스레딩
- 비동기(`Async`): 지금과 나중 사이의 간극
- 병렬(`Parallel`): 동시에 일어나는 일
- 공유 메모리에 병렬로 접근하거나 변경할 수는 없다.
- 병렬성(`Parallelism`) / 직렬성(`Serialism`)이 나뉜 스레드에서 이벤트 루프를 협동(`Cooperation`)하는 형태로 공존
- 병렬 실행 스레드 인터리빙(`Interleaving`)과 비동기 이벤트 인터리빙은 완전히 다른 수준의 단위(`Granularity`)에서 일어난다.

<br>

### 협동
> 협동적 동시성(`Cooperative Concurrency`)<br>
> 프로세스를 여러 단계/배치로 쪼개어 다른 동시 프로세스가 각자 작업을 이벤트 루프 큐에 인터리빙 하도록 하는 게 목표이다.<br>
```javascript
// 꼼수(hack)로 이벤트 루프 맨끝에 추가 - 타이머가 다음에 추가
setTimeout(function() {
    // ...
}, 0);
```

<br>

### 잡(`Job Queue`)
> 이벤트 루프 큐는 테마파크에서 롤러코스터를 타고나서 한 번 더 타고 싶어 다시 대기열 맨 끝에서 기다리는 것이고

> 잡 큐는 롤러코스터에서 내린 직후 대기열 맨 앞에서 곧바로 다시 타는 것이다.

```javascript
console.log("a");

setTimeout(function() {
    console.log("b");
}, 0);


schedule(function() {
    console.log("c");

    schedule(function() {
        console.log("d");
    });
});

// a c d b
```

<br>

### 표현식 순서
- 자바스크립트 엔진은 재정렬하면서 실행 시간을 줄일 여지는 없는지 확인
- 최종 결과가 뒤바뀌지 않도록 안전하게 최적화
- 부수효과(`Side Effect`)가 있는 함수 호출인 경우 부수효과 발생하면 안되므로 순서 조정 하지 않는다. `성능저하(ES6 프록시)`

<br>

### 기타
> 스레드 간에 데이터를 공유하는 방법이 없기에, 비결정성의 수준(`Level of Nondeterminism`)은 문제가 도지ㅣ 않는다.<br>
> 그렇다고 항상 결정적(`Deterministic`)은 아니다.
- 단일-스레드이므로 원자적(`Atomic`): 완전-실행(`Run-to-Completion`)
- 선발순(`Either-First Order`)
- 함수(이벤트)의 순서에 따른 것이지, 표현식의 순서를 따르는 것이 아니다.
- 경합조건(`Race Condition`)
- 동시성: 처리수준의 병행성(`Operation-level`) <-> 프로세스 수준의 병행성(`Process-level`)
- 관문(`Gate`) === 걸쇠(`Latch`)로 비동기 문제를 해결 했었다.



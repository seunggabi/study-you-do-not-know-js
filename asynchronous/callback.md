# 콜백 (`Callback`)
- 자바스크립트 언어에서 콜백은, 가장 기본적인 비동기 패턴
- 비결정적(`Indeterminate`) 시간 동안 중지되고, 이전 위치로 다시 돌아와서 나머지 후반부 프로그램이 이어진다.
- 연속성(`Continuation`)을 감싼(`Wrapping`)/캡슐화(`Encapsulation`)한 장치

<br>

### 사람의 두뇌
- 아주 재빠른 콘텍스트 교환기 (`Context Switcher`)
- 인간의 두뇌가 이벤트 루프 큐처럼 작동
- 단계별로 생각 (`Step-by-Step`)

<br>

### 콜백 지옥(`Callback Hell`) === 운명의 피라미드(`Pyramid of Doom`)
```javascript
do1(function() {
    do3();
    
    do4(function() {
        do6();
    });
    
    do5();
});

do2();
```

<br>

### 제어의 역전(`Inversion of Control`)
> 내가 작성하는 프로그램인데도 실행 흐름은 서드 파티에 의존해야 하는 상황
- 콜백을 너무 일찍 부른다.
- 콜백을 너무 늦게 부른다.
- 콜백을 너무 적게 또는 너무 많이 부른다.
- 필요한 환경/인자를 정상적으로 콜백에 전달하지 못한다.
- 일어날지 모를 에러/예외를 무시한다.
- ...

<br>

### 해결
- 반복적인 관용 코드 (`Boileplate`) / 오버헤드 (`Overhead`)를 넣는 식으로 필요한 장치를 만든다.
- 분할 콜백 (`Split Callback`): ES6~ 프라미스
- 에러 우선 스타일(`Error-First Style`)이라는 콜백 패턴 사용

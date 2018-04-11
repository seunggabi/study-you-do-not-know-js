# 다양한 환경의 자바스크립트

### ECMAScript
> 자바스크립트와의 호환성을 염두에 두고 공식 명세와의 특정한 차이점(`deviation`)을 기술한다.<br>
> 호환성 차이(`Compatibility Difference`)<br>
> 구분자(`delimiter`)<br>
> 자동정의변수(`Auto-Defined`)<br>
> 호스트 객체(`Host Objects`)<br>

<br>

### 전역 DOM 변수
> id 속성으로 DOM 요소를 생성해도 같은 이름을 가진 전역 변수가 생성된다는 사실은 모르는 사람들이 많다.<br>
> 충돌하지 않게 유일한 이름으로 변수를 명명하자, HTML 콘텐츠를 비롯한 다른 코드와 충돌하지 않도록 항상 확실히 점검한다.<br>

<br>

### 네이티브 프로토타입
> 여러분이 짠 코드가 특정 환경에서만 실행될 것이라는, 절대적 확신이 없는 한 네이티브를 확장하지 말자. 
> 100% 확신한다고 자신 있게 말할 수 없다면, 직접 확장한 네이티브는 위험하다. 여러분 스스로 리스크를 감수해야 한다.
```javascript
if(!Array.prototype.push) {
	// 넷스케이프 4는 Array.push 메서드가 없다.
	Array.prototype.push = function push(item) {
		this[this.length] = item;
	}
}
// (실수로 네이티브를 덮어쓸지 모르니) 이런 무차별 확장은 피하자!

```
- 건드릴 수 없는(`Untouchable`) / 전용 내장(`Private`)
- 피처(`Feature`) / 컴플라이언스(`Compliance`) 테스트
- 단위(`Unit`) / 회귀(`Regression`)

> 깨질지 모를 코드를 예상하여 여러분은 최소한의 회의적/비관적인 생각을 가져야 한다.<br>

<br>

### 심/폴리필
> 구 버전 환경은 바뀔 가능성이 거의 없기 때문에 네이티브를 확장시켜도 안전한, 유일한 장소로 꼽힌다.<br>
> 최신 명세에 미처 반영되지 못한 옛날 브라우저 환경을 패치(`Patch`)할 용도로 현재 코드 베이스에 포함시킬 수 있으니 아주 유용하다.<br>
> 폴리필은 아주 훌륭한 수단이다.<br>

- 폴리필(`Polyfill`) / 심(`Shim`)
- 프롤리필(`Prollyfill` === `Probably Fill`): 앞으로의 표준에 맞추기 위해 사전 제작된 폴리필
  - 새로운 일부 표준 기능 (완전히) 폴리필/프롤리필로 대체할 수 없다는 걸 조심하자!

> 어떤 결정을 내리든지 조심조심, 방어적인 코드를 작성하고 
> 위험 요소에 대해 가급적 문서를 잘 작성하여 여러분의 의도를 분명히 밝히기 바란다.

<br>

### `<script>`
> 각자의 자바스크립트 프로그램으로 작동한다.

<br>

### 예약어
- `Reserved Words`
  - `Keywords`
  - `Future Reserved Words`
  - `null`
  - `boolean` (true/false)
  - ...




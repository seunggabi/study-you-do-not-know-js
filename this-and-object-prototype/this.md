# `this`

> 가장 헷갈리는 체계 (`Mechanism`) 중의 하나 ...<br>
> 콘텍스트 (`Context`)<br>
> 암시적인 객체 레퍼런스를 '함께 넘기는 (`Passing Along`)' this 체계가 더 낫다.
```javascript
// 렉시컬 스코프: this property와 object property는 다르다.
function o() {
	this.count = 5;
}

o.count = 0;

o();

console.log(o.count); // 0
```

### `this`?
- 활성화 레코드 (`Activation Record`)
- 실행 콘텍스트 (`Execution Context`)
- 함수가 호출된 근원 (`Call-Stack`)과 호출 방법, 전달된 인자 등의 정보가 담겨 있다.
- `this` 레퍼런스는 그중 하나로 함수가 실행되는 동안 이용가능하다.
- `this` 바인딩을 결정짓는 함수 호출부: `Call-Site`
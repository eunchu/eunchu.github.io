---
layout: post
title: Typescript 문법 정리
categories: Typescript
tags: [frontend, typescript]
---

## Typescript

> Javascript 기반의 프로그래밍 언어<br/>
>
> Typescript 플레이그라운드 (타입스크립트 테스트)<br/>
> 🤹‍♀️ [https://www.typescriptlang.org/play](https://www.typescriptlang.org/play)

<!--truncate-->

## 기능

### 1. Optional Props

같은 속성이 다른 두 곳에서, 하나는 **필수** 하나는 **옵셔널**하게 적용될 경우 Default값을 지정할 수 있다.

```typescript
interface AppProps {
	bg: string;
	borderColor?: string; //optional한 값으로 string or undefined
}
function App({ bg, borderColor }: AppProps) {
	return (
		<Container>
			<Circle bgColor="tomato" borderColor={borderColor ?? "white"}/> //undefined일 경우 white값을 사용함
			<Circle bgColor="tomato"/>
		</Container>
	)
}

// 위와 동일함!
function App({ bg, borderColor = "white" }: AppProps) {
```

### 2. Enums

열거형 자료구조, `서로 연관된 상수들의 집할`을 정의할 수 있다.<br />
열거형을 이용하면 내용을 문서화하거나 구분되는 사례 집합을 더 쉽게 관리할 수 있다.<br />
<br />

- 타입스크립트는 숫자와 문자열 기반 열거형을 제공함

```typescript
export enum Categories {
  "TO_DO",
  "DOING",
  "DONE",
}

export const categoryState = atom<IToDo["category"]>({
  key: "category",
  default: Categories.TO_DO, // 👈
});
```

**사용시 이점**

- 관리 포인트가 한 곳이라, 수정에 용이함!
- 같은 문자 반복해서 쓸 필요도 없어지고, 에러(오타/실수) 방지할 수 있음

### 3. string to number

```typescript
1.
const a = "123";
const b = +a;
// b -> 123

2. js
Number(b);
```

---
layout: post
title: Recoil vs Redux 비교와 상태관리 라이브러리 선택
subtitle: Recoil vs Redux
categories: React
tags: [frontend, react, recoil, redux]
---

## Overview

다른 프로젝트에 투입되면서 아키텍쳐 설계부터 대대적인 수정이 필요해졌다.<br/>
또 시작되는 **리팩토링** 🥳<br/>
기존의 상태관리 방식에서 맘에 안들었던 점도 고칠겸! 이번 프로젝트의 레거시 코드도 대거 걷어낼겸<br/>
기존 프로젝트에서 사용중이었던 **[Redux](https://ko.redux.js.org/)**와 상태관리 체계가 변경되면서 사용하기 적합하다고 판단되는<br/>
**[Recoil](https://recoiljs.org/ko/)**를 비교해보고
팀원들과 공유 후 선택되는 방향으로 상태체제를 구성해보려고 한다.🙌

## Recoil vs Redux

### 1. 비교

|          | Recoil                                                                                           | Redux                                                                                   |
| -------- | ------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- |
| 컨셉     | Atomic 모델 기반<br/> : 작은 상태 단위(atom)로 관리,<br/> atom을 결합하여 데이터를 가공하는 방식 | Flux 아키텍쳐 기반<br/> : 단방향 데이터 흐름<br/> action -> dispatcher -> store -> view |
| 미들웨어 | X                                                                                                | O                                                                                       |
| 코드량   | ↑                                                                                                | ↓                                                                                       |
| 복잡도   | 중하                                                                                             | 중상                                                                                    |
| DevTools | 미흡                                                                                             | Good                                                                                    |
| 관리방식 | 분산관리                                                                                         | 중앙집중형                                                                              |

### 2. Recoil 특징

- 동시성 모드 제공
  - Concurrent mode: 흐름이 여러 개가 존재하는 경우, 렌더링 독작 우선순위를 정하여 렌더링할 수 있음
- `atom -> selector -> components` 플로우의 복잡하지 않은 구조
- 리액트를 위한 맞춤 상태관리 라이브러리
- 낮은 러닝커브
- `store`와 같은 외부 요인이 아닌 `react`내부의 상태를 활용하고 `contextAPI`를 통해 구현되어 있기 때문에 리액트 스러운 상태관리가 가능함
- 리액트 내장 `hooks`를 사용하는 방식과 거의 똑같기 때문에 쉽게 배우고 활용할 수 있음

### 3. Redux 특징

- Single source of truth
  - 같은 데이터는 항상 같은 장소에서 관리
- 상태 변경은 순수함수로만 가능함
- 브라우저 Devtools를 활용하여 현재 상태를 관리하기 편함
- 중앙집중형 store관리로 로직 복잡도가 높은 편임

---

## Recoil

### 특장점

- `atom → selector → component` 의 하나의 `data-flow`
- `Concurrent Mode` 지원
- **캐싱지원**
  - selector를 통한 비동기 작업을 진행할 때, atom값에 자동으로 의존성이 걸리는데
    atom값이 변경될 때마다 selector가 변경된다. 이때 atom의 값이 동일할 경우 내부적으로
    반환값을 메모라이즈 하고 있어 캐싱된 값을 반환함
- `useRecoilValue`를 사용하면 state, set을 반환해줌 try,catch구문을 사용하지 않아도 됨

### Atom

- 상태의 단위
- atom이 업데이트되면 해당 atom을 구독하고 있던 모든 컴포넌트의 상태가 갱신되어 리랜더됨
- unique한 key값으로 구분함

### useRecoilState

```typescript
const [value, setValue] = useRecoilState(valueState);
```

`useRecoilState` hook을 사용해서 `state`를 `get`, `set`할 수 있음<br />
**react의 useState와 동일한 사용방식**

### useRecoilValue, useSetRecoilValue

```typescript
const value = useRecoilValue(valueState);
const setValue = useSetRecoilValue(valueState);
```

value, 변경함수 각각 따로 호출하여 쓸 수 있음

### Selector

다른 selector, atom의 값을 get해올 수 있고 그 값을 바탕으로 state를 modify하는 역할

```typescript
const countState = selector({
  key: "countState", // uniqueId
  get: ({ get }) => {
    const text = get(textState);
    return text.length;
  },
  set: ({ set }, newValue) => {
    // newValue => 변경 된 상태 값
  },
});
```

- 파생된 `state`를 만들 수 있음
- `get`프로퍼티를 통해 `state`를 가공하여 반환할 수 있음
- 순수함수여야 함
- 기본적으로 값을 캐싱함 (한번 call한 api에 대해서 새로운 call이 필요 없음)

---

## 결론

1. 상태관리 변경의 필요성
2. 다른 라이브러리의 도입

   두 가지 모두 필요한 부분이고 현 프로젝트의 문제점을 해결할 수 있으며<br />
   간단한 개념으로도 쉽게 접근할 수 있는 recoil의 도입을 결정.<br />
   심지어 많은 보일러 플레이트 코드들을 줄일 수 있다.<br />
   대형 프로젝트에서의 관리에 대한 레퍼런스는 많지 않지만, 시간을 투자하여 적용할 가치가 있다고 판단됨

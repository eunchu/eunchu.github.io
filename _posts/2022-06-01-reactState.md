---
layout: post
title: 리액트 상태관리 변경에 대한 제안
subtitle: 상태구조 재설계
categories: React
tags: [frontend, react, react-query, recoil, 상태관리]
---

## Overview

**현 프로젝트 상태구조**
전역 상태 관리를 위해 `ContextAPI`를 사용하고 있으며,
로컬 상태는 각 컴포넌트에서 사용되는 상태만 각자(각 컴포넌트에서) 관리하고 있다.
프로젝트 규모가 커지면서, 컴포넌트에 로컬 상태과 비동기 통신이 혼재되고 복잡도가 높아지고 있다.
전역, 로컬, 서버 상태를 어떻게 구분하고 관리할지에 대한 설계가 필요하다고 판단되어 아래 내용을 제안합니다 🤜

> Local State: 리액트 컴포넌트 안에서만 사용되는 State<br />
> Global State: Global Store에 정의되어 프로젝트 어디서나 접근 가능한 State<br/>
> Server State: 서버로부터 받아오는 State

<!--truncate-->

## 현재 구조

**전역 상태**
![전역상태](/assets/images/post/01.png)

**로컬상태**
![로컬상태](/assets/images/post/02.png)

## ✔️ 문제점

- BA처럼 복잡한 프로젝트의 경우 백여개의 api동작마다 모두 응답값을 바라보는 상태들이 존재하고 이러한 것들이 전역 상태와 컴포넌트 내부에 산재되어 있다.
- 추가로 api에서 받은 데이터의 조작과 가공된 상태까지 존재함에따라 상태관리의 복잡도가 증가되고 있다.
  이러한 방식이 유지되면 상태관리가 어렵고, 복잡도만 늘어갈 것으로 생각됩니다.

전역 상태와 비동기 통신에 따른 상태가 분리되야하고 각 컴포넌트 내부엔 client side의 상태만
관리될 필요성이 있다고 보여짐

## 🖍 해결 포인트

- 전역 상태 관리에서 비동기 통신을 걷어내야 함
- 서버 관련 상태를 별도로 관리하되 전역 상태로 활용될 수 있어야 함
- 컴포넌트 개별단위의 상태는 유지하되 마찬가지로 비동기 통신을 걷어내야 함

## 🧐 방안 제시

**React Query의 도입**

[React Query](https://react-query.tanstack.com/)란<br/>
: Server와 client 사이 비동기 로직을 쉽게 다루게 해주는 라이브러리

### React-query 특징&강점

1. 간결한 코드

- saga처럼 비동기 관련 로직이 장황하지 않음
- `useQuery`를 통해 고유한 key값으로 구분되며, 여러 쿼리를 컴포넌트 곳곳에 뿌려둬도 동일한 데이터에 접근 가능
- 코드 길이가 짧아지고 가독성도 좋음
- 상태 저장 필요없이 바로 사용 가능

```typescript
// React-query예시
const { data, isLoading, isError, ... } = useQuery("key", fetchFc, { ...options });

return (
  <ul>
    {data.map(item => (
      <li key={item.id}>{item.name}</li>
    ))}
  </ul>
)
```

2. 동기화 및 업데이트를 보다 쉽게 처리할 수 있음

## 결론/제안

Local State: 기존 방식 유지 (useState)<br />
Global State: Recoil, Mobx같이 전역관리용 라이브러리 써치 및 도입 필요<br/>
Server State: React query를 사용하여 로컬상태와 분리함

---
layout: post
title: Next.js 라우터 정리
subtitle: Router
categories: Next.js
tags: [frontend, nextjs]
---

<!--truncate-->

![nextjs](/assets/images/post/nextjs-logo.jpeg)

## Overview

> 자주 사용되는 메서드 사용법 정리

## router.push

### 사용법

```typescript
router.push(url, as, options);
```

- url: (UrlObject | String [필수]) - 라우팅할 url
- as: (UrlObject | String) - 브라우저 url에 표시될 경로
- options
  - scroll: 라우팅 이동 후 페이지 스크롤 제어(맨 위). 기본값 `true`
  - shallow: 재실행 없이 현제 페이지의 경로 업데이트. 기본값 `false`
  - locale: 새 페이지의 로케일 표시

### 예시

```typescript
import { useRouter } from 'next/router

export default function Component() {
  const router = useRouter()

  return (
    <div onClick={() => router.push('/')}>클릭하면 이동</div>
  )
}
```

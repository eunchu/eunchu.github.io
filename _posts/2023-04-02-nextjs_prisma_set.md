---
layout: post
title: Next.js + Prisma + mongoDB 연결 오류
subtitle: 오류 픽스
categories: Next.js
tags: [frontend, nextjs, prisma, mongodb]
---

<!--truncate-->

## Overview

> 풀스텍을 향하여-! 👩‍💻

> 까먹지말고 기록해두자

## 연결부터 해결하자

공홈+블로그 보며 연결을 시도했으나..
mongodb만 연결해서 쓸때는 잘만되던게 error를 밷어낸다

## 연결

> npx prisma init

위 명령어를 통해 prisma를 초기화하면,

프로젝트 내부에 아래처럼 prisma 폴더를 만든 뒤 새 파일이 생긴다

![01](/assets/images/post/0402_01.png)

주요 prisma 구성 파일이며, 아래의 3가지 요소가 포함된다

- 데이터소스
- 데이터 모델이 생성 될 위치 지정
- 데이터 모델

아래처럼 셋팅 후,
user모델도 추가하고 ...

![02](/assets/images/post/0402_02.png)

> npx prisma db push

명령어를 통해 데이터베이스에 배포를 실행하면, 이때 오류가 발생하는데

쪼꼼 당황했으나.. 역시 공식문서를 살펴보니 `.env` 에 작성해둔 `DATABASE_URL`에 빠진 부분이 있었다

```
"mongodb+srv://root:<password>@cluster0.ab1cd.mongodb.net/[여기]?retryWrites=true&w=majority"
```

[여기] 부분에 db이름이 들어가 있었고.. 😵

이름 추가 후 다시 배포하니 아주 잘 되었다고 한다;)...

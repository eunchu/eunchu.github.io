---
layout: post
title: NVM으로 프로젝트 별 Node버전 관리하기
subtitle: 노드 버전 관리
categories: Tip
tags: [node, tip]
---

여러 프로젝트를 동시에 진행하다보니, 프로젝트 별 Node버전을 관리해야 될 필요성이 생겼다.
요번 포스팅에선 맥유저를 위해 NVM으로 Node버전을 관리하는 방법에 대해 알아보겠습니다.

### NVM 설치

```
brew install nvm
```

### 명령어

```
// 설치 가능한 Node버전 체크
nvm ls-remote

// 최신 버전 Node 설치
nvm install node

// LTS release 버전 설치
nvm install --lts

// 특정 버전 Node 설치
nvm install 12.13.0

// 설치된 Node 버전 체크
nvm ls

// 버전 삭제
nvm uninstall 12.13.0
```

위의 명령어로 대부분의 처리가 가능합니다 😀

### 버전 변경

사용할 노드의 여러 버전들을 설치했다면 각 프로젝트에서<br/>
사용하고자 하는 버전을 선택해서 이용할 수 있습니다.

먼저, .nvmrc파일을 생성하여 해당 프로젝트에서 사용될 버전을 적어줍니다.

```
12.13.0
```

다음, 아래 명령어를 통해 .nvmrc에 기입된 버전으로 변경하여 사용합니다.

```
nvm use
```

.nvmrc파일은 .gitignore에 추가하여 개인적으로 사용해도 되고,
팀원들과 공유하여 사용해도 됩니다.

---
repository: https://github.com/chanooda/fsd-next
tags:
  - nextjs
  - fsd
  - pocketbase
created_at: 2024-12-04
update_at: 2024-12-04
---
[FSD 튜토리얼 프로젝트](https://feature-sliced.design/kr/docs/get-started/tutorial)를 재구성하며 공부해보기 

---
FSD 공홈에 있는 튜토리얼 프로젝트는 Remix 기반, app, pages, shared 레이어만을 가진 간단한 구조로 구성되어 있다. FSD에 대해서 좀 더 자세히 알아보기 위해 고도화를 진행

튜토리얼에서 제공하는 api 서버가 동작하지 않아서 pocketbase를 이용해서 간단하게 백엔드 구성
프론트엔드는 next.js를 이용해 개발
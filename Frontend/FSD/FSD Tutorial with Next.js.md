---
repository: https://github.com/chanooda/fsd-next
tags:
  - nextjs
  - fsd
  - pocketbase
created_at: 2024-12-04
---
[FSD 튜토리얼 프로젝트](https://feature-sliced.design/kr/docs/get-started/tutorial)를 재구성하며 공부해보기 

---
FSD 공홈에 있는 튜토리얼 프로젝트는 Remix 기반, app, pages, shared 레이어만을 가진 간단한 구조로 구성되어 있다. FSD에 대해서 좀 더 자세히 알아보기 위해 고도화를 진행

튜토리얼에서 제공하는 api 서버가 동작하지 않아서 pocketbase를 이용해서 간단하게 백엔드 구성
프론트엔드는 next.js를 이용해 개발

[[프론트엔드 프로젝트 아키텍처에 대한 고찰]] 기반으로 프로젝트 진행

## 진행
### shared
- `shared/api` api를 직접적으로 통신하는 함수들을 작성했다. api-generator에서 만들어지는 폴더로 생각해도 될 거 같다.
	- api 통신 함수 (axios.get, axios.post 등등)
- `shared/model` api request, response, error 타입을 관리
	- 전역적으로 사용되는 schema
- `shared/config` 백엔드 관련, sdk 클라이언트 관련 함수와 상수 관리
	- axios client 관련 코드
- `shared/libs` useMutation, useQuery 등 tanstack-query 관련 래핑 함수를 관리
	- 글로벌로 사용되는 라이브러리 관련 코드
	- global하게 사용되는 util 함수 (dayjs, lodash, validation, format)
- `shared/ui` shadcn 컴포넌트와 에러메시지 등 제일 작은 단위의 컴포넌트 관리
	- 전역적으로 사용되는 작은 단위의 컴포넌트
	- 디자인 시스템 라이브러리 혹은 디자인 시스템 컴포넌트 관련 컴포넌트
### entities
- `entities/(slice)/api` 
	- api 함수와 직접적으로 사용되는 함수들  (tanstack-query
	- api 함수의 response, request, error 타입 (client side의 느낌 한번 래핑한다는 느낌)
	- mapper 함수



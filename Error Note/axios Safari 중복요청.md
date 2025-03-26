## 문제
Safari 브라우저에서만 페이지 로드가 안됨

## 환경
Next.js v14.3.2
axios v1.5.1
@tanstack/react-query v5.13.4

## 원인 파악 
1. 우선 server component에서 prefetchQuery를 사용하면 페이지 로드가 안되는 것을 파악 (사용하지 않은 라우트에서는 페이지 로드가 잘 동작함)
2. prefetch 하는 함수 내부에 console.log를 찍어본 결과, 페이지 로드 안될 때에는 첫 api 호출이 실패하고 두번째 api 호출로 이어지는 것을 확인 그러나 `try/catch` 로 에러가 잡히지 않음
3. api 호출하는 과정에서 axios 내부에 문제가 있을 것을 예상 error interceptor에서 error를 찍어본 결과 api 중복호출을 방지하는 interceptor에서 error가 잡힌는 걸 확인 
4. api 중복호출 에러를 처리하는 과정에서 `return new Promise(() => {});` 처리를 하는 과정에서 페이지 로드가 막히는 것 같음
```ts
private _responseInterceptorOnRejected = (error: AxiosError<unknown>) => {

this.requests.delete(error.config?.url || '');

if (axios.isCancel(error)) return new Promise(() => {});

return Promise.reject(error);

};
```
5. `return new Promise()` 가 아니라 그냥 `return` 을 하거나 `throw error` 를 하면 페이지 로드가 안되는 현상은 일어나지 않으나 server side에서 data cache가 안됨 (tanstack/react-query에서는 최종적으로 중복 호출된 undefined 값으로 캐시 처리를 하는 것 같음)
## 정리 및 결론
복합적인 문제
1. safari 브라우저에서 server component 내에서 axios를 통해 api를 호출하면 중복 호출이 발생함 (`fetch` 를 사용하면 중복호출 발생하지 않음 axios문제인지 next.js 문제인지 확인 필요 )
2. 중복호출 에러를 처리하는 과정에서 `return new Promise(()=>{})` 를 통해 처리하는데 이 부분에서 페이지 로드가 막히는 것 같음 tanstack/react-query와 next.js에서 캐시를 처리하는 과정에서 오류가 발생하는 것 아닐까 추측
3. 직접적인 이유는 중복호출을 처리하는 interceptor의 에러처리 로직에서 발생
    간접적인 이유는 safari 브라우저에서만 server component에서 axios를 사용하면 api가 중복 발송되는 것
## 해결
우선 server component에서 prefetch 할 때에는 `fetch` 를 사용하도록 함 (api 중복호출을 방지하는 걸로 해결) 

## 관련 이슈
https://github.com/axios/axios/pull/6584
next.js에서 제공하는 cookies 함수 cookie에 접근할 수 있다. 

> [!warning]
> `cookie.set` 은 sercer action 이나 route handler에서 사용가능 

### http-only cookie 
next.js의 server-side에서는 http-only cookie에 접근할 수 있다. 

> [!warning]
> 외부 api 서버에서 client에 꽂아주는 쿠키가 http-only cookie이면서, path 설정이 걸려있다면 접근할 수 없다. 도메인과 path가 같아야지만 해당 http-only cookie를 발송한다. 
> 도메인과 path가 다른 next.js쪽 server-side에 브라우저가 요청을 보낼때 http-only cookie를 보내지 않기 때문에 접근하지 못함







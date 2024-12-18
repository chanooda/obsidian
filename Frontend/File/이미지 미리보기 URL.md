```js
const objectURL = URL.createObjectURL(object);
```

인자에 file 객체를 넘겨서 미리보기 url 생성할 수 있다.

`createObjectUrl`은 호출 때마다 새로운 객체 URL을 생성하기 때문에 사용하지 않을 때에는 `URL.revokeObjectURL` 을 사용해 제거해줘야 한다.  

---
- https://developer.mozilla.org/ko/docs/Web/API/URL/createObjectURL_static
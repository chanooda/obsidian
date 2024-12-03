기존에 사용되는 `typeof`는 원시타입 이외에는 정확한 구분을 하기 힘들었다. (array, object, function, Date 등)

`Object.prototype.toString.call` 메서드를 사용하면 더 세부적인 타입을 알아낼 수 있다.

|                                            |                     |
| ------------------------------------------ | ------------------- |
| `Object.prototype.toString.call(new Date)` | `[object Date]`     |
| `Object.prototype.toString.call([])`       | `[object Array]`    |
| `Object.prototype.toString.call(()=>{})`   | `[object Function]` |

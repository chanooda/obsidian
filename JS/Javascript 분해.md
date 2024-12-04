자바스크립트의 이론적인 부분을 되짚어보거나 배우기 위해 정리

---

# JavaScript Runtime

JavaScript Runtime은 브라우저 상에서 실행되는 JavaScript 환경이다.

JavaScript의 런타임 모델은 코드의 실행, 이벤트의 수집과 처리, 큐에 대기중인 하위 작업을 처리하는 이벤트 루프에 기반하고 있다. 

JavaScript Runtime은 브라우저에 내장되어 있는 JavaScript Engine과 그 밖의 여러 기능들이 합쳐져 이루어진다.

## JavaScript Engine

JavaScript 코드를 실행하는 프로그램 또는 인터프리터이며, 코드를 컴파일하는 과정 없이 즉시 실행시킨다. JavaScript는 하나의 Stack을 가지고 작업을 처리하며, 이를 보면 JavaScript는 한번의 하나의 작업, **동기적(synchronous)**, **싱글 쓰레드(single thread)**로 동작하는 것을 알 수 있다.

JavaScript Engine은 각각 하나의 Heap과 Stack으로 이루어져있다. 

## Call Stack

**call stack** 혹은 **execution context stack** 이라고 불리며 다음과 같은 역할을 한다.

- 코드가 실행될 때마다 필요한 환경과 정보들을 제공하는 **실행 컨텍스트(execution context)**를 관리한다.
- 정적 데이터, 원시 타입 데이터(string, numbrer, boolean, undefined, null 등)의 값들을 메모리에 저장한다.

실행 컨텍스트는 전역에서 기본적으로 생성되고, 함수가 호출되거나  block scope(if문, for문, 그냥 {})를 만나면 생성되어 Stack의 맨 위에 쌓이게 된다. 

만약 실행 컨텍스트가 실행중에, 새로운 실행 컨텍스트가 생성되면 기존의 실행 컨텍스트는 작업을 멈추며 새롭게 생성된 실행 컨텍스트가 실행된다. 

 Stack은 맨 위부터 아래까지 순서대로 실행 컨텍스트를 실행하게 된다.(Stack의  LIFO 특성에 맞추어)

## **실행 컨텍스트(execution context)**

실행 될 코드에 필요한 환경과 정보를 제공하는 객체이다. 

### 실행 컨텍스트 주요 구성 요소

- **Variable Environment**
    
    **Lexical Environment**와 같은 내용이 담기지만, 최초 실행 시의 스냅샷을 유지한다.
    
- **Lexical Environment**
    - **Enviroment Record (환경 레코드)**
        - **Declaratice Enviroment Record**
            
            현재 범위 내의 var, const, let, class, module, import/exrpot, and/or, function 등의 선언된 식별자 정보를 보유한다. 실행 과정에서 업데이트 되거나 참조되는 식별자와 값을 바인딩 해준다.
            
            - **Function Environment Records**
                
                함수의 로컬 변수 및 매개 변수의 선언된 식별자 정보를 저장하고 식별자의 값과 바인딩 해준다. 함수 환경 레코드는 매개 변수로 초기화되며 로컬 변수들이 추가된다.
                
            - **Module Environment Records**
                
                모듈의 export/import, 로컬 변수에 대한 정보를 저장한다.
                
        - **Object Environment Records**
            
            binding object와 연결된다. binding object의 속성에 해당하는 문자열 식별자를 바인딩한다.
            
        - **Global Environment Record**
            
            전역 범위에서 정의된 모든 변수와 함수를 추적하는 데 사용된다. 프로그램의 전역 범위를 나타내며, **Declaratice Enviroment Record**와 **Object Environment Records**를 포함하고 있다.
            
    - **Outer Enviroment Reference**
        
        **스코프**와 **스코프체인**을 형성하며 외부의 **Lexical Environment**에 접근하여 식별자의 유효범위를 파악한다.
        
        - **스코프** : 식별자에 대한 유효 범위
        - **스코프 체인** : 식별자의 유효범위를 안에서 바깥으로 차례로 검색해 나가기 위한 연결 리스트

### 실행 컨텍스트의 단계

실행 컨텍스트의 실행은 2가지의 단계로 나누어져 있다.

- 생성 단계 (Creation Phase) - 선언 식별자들을 환경 레코드에 기록
- 실행 단계 (Execution Phase) - 선언문 외 나머지 코드 순차적 실행, 선언 식별자들의 바인딩 작업이 이루어짐

## Memory Heap

동적 메모리를 할당하며 객체와 함수를 저장한다. 스택에서는 원시 타입의 정적 데이터를 저장하고 스택의 메모리를 사용한다. 메모리 힙에서는 배열, 객체, 함수 등 참조 타입의 동적 데이터를 저장하고 콜스택에서 메모리 힙의 주소를 이용해 연결한다.

## Callback Queue

콜백 함수의 대기열이며, Javascript Runtime에서 비동기 함수들의 콜백 함수 처리를 담당한다. Call Stack에서 비동기 함수를 만나면 브라우저에 넘기고, 브라우저는 별도의 쓰레드에 해당 함수를 처리하고 종료되면 전달받은 콜백 함수를 Callback Queue에 전달한다.

Callback Queue에는 여러 종류가 있다.

- **Task Queue** - 이벤트 핸들러, setTimeOut, fetch 같은 일반적인 비동기 함수의 콜백 함수가 들어온다.
- **Microtask Queue -** promise, async/await 같은 비동기 함수의 콜백 함수가 관리하며 Task Queue보다 우선순위가 높다.

## 이벤트 루프 (Event Loop)

이벤트 루프는 Non-blocking 방식으로 비동기 작업을 처리하기 위해 사용된다. 이벤트 루프는 매 순간 Call Stack이 비어있는지 확인하여, Call Stack이 비게 되면 Callback Queue에 처음으로 들어온 콜백 함수를 Call Stack에 추가한다.

# JavaScript의 동기 비동기

비동기 프로그래밍은 작업이 완료될 때까지 기다리지 않고 잠재적으로 오래 실행되는 작업을 시작하여  해당 작업이 실행되는 동안에도 다른 이벤트에 응답할 수 있게 하는 기술입니다. 

자바스크립트 내에서 비동기로 동작하는 것들은 대표적으로 다음과 같다.

- fetch
- getUserMedia
- setTimeOut

## 동기 프로그래밍

```jsx
const name = 'Miriam';
const greeting = `Hello, my name is ${name}!`;
console.log(greeting);
// "Hello, my name is Miriam!"
```

자바스크립트는 기본적으로 동기로 작동하며, 위 코드는 위에서 아래로 차례대로 실행된다. 이전 줄의 작업이 종료되지 않는다면 다음 줄의 코드는 동작하지 않는다.

이것이 동기 프로그래밍이다.

## 장기 실행 동기 함수

동기 함수가 오랜 시간에 걸쳐 실행되면 어떨까?

```html
<label for="quota">Number of primes:</label>
<input type="text" id="quota" name="quota" value="1000000">

<button id="generate">Generate primes</button>
<button id="reload">Reload</button>

<div id="output"></div>
```

```jsx
function generatePrimes(quota) {

  function isPrime(n) {
    for (let c = 2; c <= Math.sqrt(n); ++c) {
      if (n % c === 0) {
          return false;
       }
    }
    return true;
  }

  const primes = [];
  const maximum = 1000000;

  while (primes.length < quota) {
    const candidate = Math.floor(Math.random() * (maximum + 1));
    if (isPrime(candidate)) {
      primes.push(candidate);
    }
  }

  return primes;
}

document.querySelector('#generate').addEventListener('click', () => {
  const quota = document.querySelector('#quota').value;
  const primes = generatePrimes(quota);
  document.querySelector('#output').textContent = `Finished generating ${quota} primes!`;
});

document.querySelector('#reload').addEventListener('click', () => {
  document.location.reload()
});
```

이는 **동기 함수의 기본적인 문제**를 나타낸다. 이를 해결하기 위해 우리가 필요한 것은

1. 함수를 호출함으로써 장기적으로 실행되는 작업을 시작한다.
2. 함수 호출에 의해 작업을 시작한 즉시 다른 작업을 실행할 수 있게 한다. (이벤트의 대한 응답 등)
3. 장기적으로 실행된 작업이 완료되면 결과를 알려준다.

위와 같은 문제를 해결할 수 있는 것이 **비동기 함수**이다.

## 동기 작업을 비동기화

위의  작업을 비동기화 해보자 `generatePrimes` 함수를 `setTimeout` 안에 callback 함수로 사용하여 비동기로 작동하게 했다. 이전 작업과 다른 점은 Generate primes 버튼을 누르면 메시지가 출력되고 그 뒤에 `generatePrimes` 함수를 실행시키는 것을 알 수가 있다. 

setTiemOut은 브라우저 web api의 비동기 함수기 때문에 setTimeOut의 인자로 넘어간 콜백함수는 Callback Queue인 Task Queue에 전달되고 eventLoop는 동기 작업이 모두 끝난 Call Stack에 콜백 함수를 전달하고 실행된다.

그러나 여전히 함수가 동작할 동안에는 마우스 클릭이나 input에 focus를 둘 수가 없다. 

```jsx
document.querySelector("#generate").addEventListener("click", () => {
  const quota = document.querySelector("#quota").value;
  const primes = setTimeout(() => generatePrimes(quota), 0);

  document.querySelector(
    "#output"
  ).textContent = `Finished generating ${quota} primes!`;
});

document.querySelector("#reload").addEventListener("click", () => {
  document.location.reload();
});
```

https://codesandbox.io/embed/relaxed-faraday-efj1ry?fontsize=14&hidenavigation=1&theme=dark

# **Reference**

---

- JavaScript Runtime에 대한 이해
    - https://www.youtube.com/watch?v=8aGhZQkoFbQ
    - https://developer.mozilla.org/ko/docs/Web/JavaScript/EventLoop
- 자바스크립트 상에서 메모리적인 stack과 Heap의 역할과 차이
    - [https://erwinousy.medium.com/javascript의-메모리-관리법-f943bddfd5fb](https://erwinousy.medium.com/javascript%EC%9D%98-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B4%80%EB%A6%AC%EB%B2%95-f943bddfd5fb)
    - https://charming-kyu.tistory.com/19
- stack 상에서 frame과 실행 컨텍스트에 대한 차이를 알기 위해
    - https://onlydev.tistory.com/158
    - https://leehyungi0622.github.io/2021/04/22/202104/210422-javascript_stack_frame_and_execution_context/
- JavaScript Engine에 대한 이해
    - [https://ko.wikipedia.org/wiki/자바스크립트_엔진](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8_%EC%97%94%EC%A7%84)
- 실행 컨텍스트에 대한 이해
    - https://catsbi.oopy.io/fffa6930-ca30-4f7e-88b6-28011fde5867
    - https://www.youtube.com/watch?v=EWfujNzSUmw
    - https://tc39.es/ecma262/#sec-executable-code-and-execution-contexts
    - https://www.youtube.com/watch?v=pfQfEwnJHRs
- block scope에서의 실행 컨텍스트를 알기 위해
    - https://www.zerocho.com/category/JavaScript/post/609778ad9f879900043a8728
- 동기와 비동기에 대한 이해
    - https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Introducing
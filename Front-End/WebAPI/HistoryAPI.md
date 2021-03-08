# History Api

>실전 리액트 프로그래밍 책 1장에서 react-router-dom을 설명하면서, 이 패키지 없이 직접 SPA 의 라우터를 구현하는 실습 코드를 제시했다. 그러면서 History  API를 좀 더 자세히 알아보기로 했다.



MDN 공식 문서를 살펴보면 (https://developer.mozilla.org/ko/docs/Web/API/History)



#### 정의

**`History`** 인터페이스는 **브라우저의 세션 기록**, 즉 현재 페이지를 불러온 탭 또는 프레임의 방문 기록을 조작할 수 있는 방법을 제공합니다.



#### 속성

[`History.length`](https://developer.mozilla.org/ko/docs/Web/API/History/length) Read only

현재 페이지를 포함해, 세션 기록의 길이를 나타내는 정수를 반환합니다.

[`History.scrollRestoration`](https://developer.mozilla.org/ko/docs/Web/API/History/scrollRestoration)

기록 탐색 시 스크롤 위치 복원 여부를 명시할 수 있습니다. 가능한 값은 `auto`와 `manual`입니다.

[`History.state`](https://developer.mozilla.org/ko/docs/Web/API/History/state) Read only

**기록 스택 최상단의 스테이트**를 나타내는 값을 반환합니다. `popstate` 이벤트를 기다리지 않고 현재 기록의 스테이트를 볼 수 있는 방법입니다.



#### 메소드

[`History.back()`](https://developer.mozilla.org/ko/docs/Web/API/History/back)

세션 기록의 바로 뒤 페이지로 이동하는 비동기 메서드입니다. 브라우저의 뒤로 가기 버튼을 눌렀을 때, 그리고 `history.go(-1)`을 사용했을 때와 같습니다.



[`History.forward()`](https://developer.mozilla.org/ko/docs/Web/API/History/forward)

세션 기록의 바로 앞 페이지로 이동하는 비동기 메서드입니다. 브라우저의 앞으로 가기 버튼을 눌렀을 때, 그리고 `history.go(1)`을 사용했을 때와 같습니다.



[`History.go()`](https://developer.mozilla.org/ko/docs/Web/API/History/go)

현재 페이지를 기준으로, 상대적인 위치에 존재하는 세션 기록 내 페이지로 이동하는 비동기 메서드입니다. 예를 들어, 매개변수로 `-1`을 제공하면 바로 뒤로, `1`을 제공하면 바로 앞으로 이동합니다. 세션 기록의 범위를 벗어나는 값을 제공하면 아무 일도 일어나지 않습니다. 매개변수를 제공하지 않거나, `0`을 제공하면 현재 페이지를 다시 불러옵니다.



[`History.pushState()`](https://developer.mozilla.org/ko/docs/Web/API/History/pushState)

주어진 데이터를 지정한 제목(제공한 경우 URL도)으로 세션 기록 스택에 넣습니다. 데이터는 DOM이 불투명(opaque)하게 취급하므로, 직렬화 가능한 모든 JavaScript 객체를 사용할 수 있습니다. 참고로, Safari를 제외한 모든 브라우저는 `title` 매개변수를 무시합니다.



[`History.replaceState()`](https://developer.mozilla.org/ko/docs/Web/API/History/replaceState)

세션 기록 스택의 제일 최근 항목을 주어진 데이터, 지정한 제목 및 URL로 대체합니다. 데이터는 DOM이 불투명(opaque)하게 취급하므로, 직렬화 가능한 모든 JavaScript 객체를 사용할 수 있습니다. 참고로, Safari를 제외한 모든 브라우저는 `title` 매개변수를 무시합니다.



중점적으로 알아볼 것은 History API의 메소드들이다.

back, forward는 go로 똑같이 동작할 수 있고, go는 history 기록을 앞뒤로 이동하면서 페이지를 로딩하는 메소드이다.

좀 특이한 것은 pushState와 replaceState이다.

replaceState는 pushState와 다른점이 추가 대신 교체되는 것 밖에 없으므로 pushState를 자세히 보겠다.



MDN의 History.pushState  공식문서(https://developer.mozilla.org/ko/docs/Web/API/History/pushState)



구문

```javascript
history.pushState(state, title[, url]);
```

파라미터는 세 개가 있다.

1. state는 각 히스토리 엔트리가 가지는 state 객체를 말한다. 직렬화(지금은 대충 문자열화 라고 치자) 가능한 모든 객체는 가능하다. 이걸로 페이지마다 다른 state를 가지게 할 수 있어서 SPA에서 아주 유용하다.

2. title은 사파리 말고는 모든 브라우저가 지원을 안한다... 그냥 빈 문자열을 넘기면 된다. 

3. url로 현재 히스토리 엔트리와 origin이 같은 다른 경로를 전달할 수 있다.

   (**중요**) pushState로 추가된 히스토리 엔트리로 접근된 url은, 브라우저가 실제로 그 경로가 존재하는지 확인하거나, 로드를 시도하지 않는다. 이래서  SPA에서 이걸 쓰는것이다. 

   

   아래 코드를 브라우저에서 열어보면 이해가 빠를것이다.

   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta http-equiv="X-UA-Compatible" content="IE=edge" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <style>
         section {
           margin: 20px 0;
         }
       </style>
       <title>Document</title>
     </head>
     <body>
       <section>
         <h2>push state</h2>
         <button id="push-state1">pushState1</button>
         <button id="push-state2">pushState2</button>
         <button id="push-state3">pushState3</button>
       </section>
       <section>
         <h2>replace state</h2>
         <button id="replace-state">replaceState</button>
       </section>
       <section>
         <h2>history state</h2>
         <span id="state"></span>
       </section>
       <script>
         // 현재의 history state 값을 출력합니다.
         const currentHistoryState = () => {
           document.getElementById("state").innerText = JSON.stringify(
             history.state
           );
         };
   
         currentHistoryState();
   
         // pushState()
         document.getElementById("push-state1").addEventListener("click", () => {
           history.pushState({ data: "pushState1" }, "", "/push-state1");
           currentHistoryState();
         });
         document.getElementById("push-state2").addEventListener("click", () => {
           history.pushState({ data: "pushState2" }, "", "/push-state2");
           currentHistoryState();
         });
         document.getElementById("push-state3").addEventListener("click", () => {
           history.pushState({ data: "pushState3" }, "", "/push-state3");
           currentHistoryState();
         });
   
         // replaceState()
         document.getElementById("replace-state").addEventListener("click", () => {
           history.replaceState({ data: "replaceState" }, "", "/replace-state");
           currentHistoryState();
         });
   
         // 브라우저의 뒤로가기 / 앞으로가기를 누르면 history state 값을 확인하여 출력합니다.
         window.addEventListener("popstate", () => {
           console.log("popstate!");
           currentHistoryState();
         });
       </script>
     </body>
   </html>
   
   ```

   push-state 버튼들을 누르면 pushState가 일어나고, currentHistoryState로 현재 history state를 출력한다.

   

   (**중요**) 

   언뜻 보면 pushState와 replaceState가 popstate 이벤트를 발생시키는것 같지만 아니다.

   popstate 이벤트는 활성화된 히스토리 엔트리에 변화가 있을 때마다 발생한다. 

   뒤로가기와 앞으로가기(history의 go, back, forward 메소드)가 그 예이다.

   
   
   popState 이벤트의 state는 해당 히스토리 엔트리에 등록되어있는 history state의 복사본이다. 이걸로 SPA에서 서버 요청 없이 이전 화면을 보여줄 수 있게 한다.
   
   ​		



#### 정리



go, back, forward 는 이미 생성된 히스토리 안에서 이동하는 것이다. (with popstate)

popstate 이벤트에서 state를 사용해 서버 요청없이 뒤로가기, 앞으로가기 등을 구현한다.(SPA)



pushState, repalceState는 새 히스토리 엔트리를 만드는 것이다! (without popstate)

서버 요청없이(실제로 로드를 시도하지 않으므로) 새 페이지에 들어가는 것 같은 효과를 만들 수 있다. (SPA)






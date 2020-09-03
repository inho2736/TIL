# Firebase 뒤로가기 에서 Cache된 값 가져오기

> 현재 개발중인 react 앱은 firebase를 이용하여, 별다른 서버를 구현하여 쓰지는 않고 있다.
>
> 하지만 firebase의 트래픽이 걱정되어, 리액트에서 cache를 어떻게 사용할 지에 대한 고민이 불가피해졌다.



처음엔 단순히 "react 에서 cache 쓰기" 라는 키워드를 이용하여 방법을 찾아보았다.

SWR과 같이 next.js 개발자들이 만든 효율적인 라이브러리도 있었고, 

이외에도 axios-extension을 이용해 캐시할 수 있는 라이브러리도 있었는데 이 라이브러리들은 전부 REST API 를 쓸 때만 사용 가능해서 일단 내가 쓸 수 없었다. (firebase는 자체 라이브러리로 서버와 통신)



아쉬운 마음을 뒤로하고 그렇다면 firebase에서는 어떻게 캐시된 값을 가져올 수 있을지 찾아보았다.

```javascript
firebase.firestore().enablePersistence()
  .catch(function(err) {
      if (err.code == 'failed-precondition') {
          // Multiple tabs open, persistence can only be enabled
          // in one tab at a a time.
          // ...
      } else if (err.code == 'unimplemented') {
          // The current browser does not support all of the
          // features required to enable persistence
          // ...
      }
  });
// Subsequent queries will use persistence, if it was enabled successfully
```



사용하는 firestore에 enablePersistence 메서드를 호출하면 오프라인 지속성이 활성화된다.

그러면 사용자가 오프라인 상태일 때는 전부 캐시값을 사용한다.



대신 내가 쓰는 웹의 경우에는 뒤로가기나 앞으로 가기일 때, 사용자의 네트워크 엑세스를 잠시 사용 중지하고 캐시 값을 받아올 필요가 있을 것 같다.





### 컴포넌트 마운트 시 이동 경로 파악하기



실제로 캐시를 쓰는 문제는 뒤로하고, 이제 페이지가 마운트 될 때 새로고침이나 링크를 통해서 이동했는지 아니면 뒤로가기나 앞으로 가기를 통해 이동했는지를 구별해야 했다. 



기본적으로 react-router 에서 제공하는 history 를 통해서 링크들간의 이동을 파악할 수 있다.

history.action이 "POP" 이면 뒤로가기, 앞으로가기, 새로고침 셋 중 하나이고, 

"PUSH" 이면 링크를 눌러 이동한 것이다.



내가 원한 것은 뒤로가기, 앞으로 가기를 통해 url을 이동한 경우는 cache 데이터를

새로고침을 누르거나, 링크를 눌러 이동한 경우는 서버에서 진짜 데이터를 가져오기를 바랬다.



링크를 눌러 이동하는 것은 쉬우나, 가장 큰 문제는

**새로고침과 뒤로가기, 앞으로가기 를 구별하는 것** 이었다



#### 첫 번째 아이디어



각 페이지 로딩의 useEffect에 history의 action을 확인 하는게 아니고

전체 app에 useEffect에 listen을 달아 뒤로가기나 앞으로가기 시 리덕스 값 true -> false로

각 페이지 로딩useEffect에서는 리덕스 값이 참일 때만 데이터 fetch



->  첫 번째 아이디어 결과 :  실패 

 usehistory() 의 history를 사용하려면 그 사용하는 컴포넌트가 라우터 안에 들어가 있어야 한다.

근데 app.js에서 라우터로 감싸는 거니까, app 컴포넌트는 라우터 안에 들어가 있지 않아서 history를 사용할 수 없다.



#### 두 번째 아이디어



각 페이지의 useEffect에서 history의 listen으로 뒤로가기, 앞으로가기를 판단하여 리덕스에 저장

( 확인할 것 : 판단 될 때, 콘솔이 몇번 찍히는지. 지난번 unmount때 판단해 콘솔찍는 것은 뒤로가기를 반복하면 누적해서 콘솔이 4회 5회 찍히는 경우도 발생했음. 불필요한 반복이 일어나진 않는지 확인 필요 -> listen을 빼고 그냥 history의 action만 확인하니 해결되었다.)



그리고 그 useEffect안에서 리덕스 캐시가 참이 아니면 일반 데이터 fetch. 참이면 캐시 데이터 fetch



-> 두 번째 아이디어 결과 : 실패

각 페이지의 useEffect에서 마운트 될때마다, action으로 뒤로가기 앞으로가기를 판단 할 수 있지 않을까? 했는데

```javascript
useEffect(() => {
    if (history.action === "POP") {
      console.log("cache");
     // 값에 캐시임을 알려서 훅 리턴값으로 가지기
    } else {
      console.log("fetch");
     // 값에 캐시가 아님을 알려서 훅 리턴값으로 가지기      
    }   
    // eslint-disable-next-line
  }, []);
};
```

이렇게 하면 새로고침 이벤트가 pop이므로, 맨 처음 페이지에 들어갔을 때도 캐시값이 들어간다. 



#### 세 번째 아이디어



맨 처음 페이지에 들어갔을 때 캐시값을 가져오는 것을 방지하기 위해서는,

 페이지가 언마운트 되었을 때마다 다음페이지로 가는 history의 action을 감지해야 한다.



다음 페이지 컴포넌트한테 그 액션을 알릴 방법이 없으므로 리덕스에 값을 저장하고,

다음 페이지에서는 렌더링 전에 리덕스 값을 확인하여 캐시값을 가져올 것인지, 새로 데이터를 받아올 것인지 선택한다.



```javascript
useEffect(() => {
    return () => {
      if (history.action === "POP") {
        // console.log("cache");
        dispatch(
          setCache({
            noCache: false
          })
        );
      } else {
        // console.log("render");
        dispatch(
          setCache({
            noCache: true
          })
        );
      }
    };
    // eslint-disable-next-line
  }, []);
```



-> 세 번째 아이디어 결과 : 성공

모든 페이지에 저 useEffect를 걸면 뒤로가기와 앞으로가기를 할 때는 리덕스에 false를, 

새로 링크를 타고 이동하면 리덕스에 true를 저장했다.



새로고침(과 맨 처음 렌더링)을 하면 저 useEffect는 언마운트 시에만 작동하므로 작동하지 않으니

리덕스의 기본값을 true로 지정해 두었다.



이런식으로 하면  이동 경로를 잘 구별한다. 

문제는 리덕스 값으로 새 페이지에서 캐시값을 가져올 지, 서버에서 새로 데이터를 가져올 지 결정하는 코드에서 일어났다.



```javascript
const noCache = useSelector(state => state.cache.noCache);
  useEffect(() => {
    if (noCache) {
      console.log("fetch");
    }
     else {
      console.log("cache");
    }
  }, [noCache]);
```

**A page**를 처음 가면 리덕스의 noCache값은 **true**이다. 

그 다음 링크를 통해  **B page**로 이동하면 거기서도 리덕스의 noCache값은 **true** 이다.

**B page**에서 뒤로가기를 눌러 다시 **A page**로 이동하면,

 **B page**가 unmount될 때 리덕스 noCache값을 **false**로 바꾼다.



그래서 A page에서는 useEffect로 리덕스 noCache의 false값을 인식해 캐시 값만 가져와야 한다. (내 예상)

하지만 리덕스의 값 변화는 한박자 늦게(?) 일어난다.

A page의 useEffect가 리덕스값이 true에서 false로 변한 후 실행되는 것이 아니라, 변하는 도중에 실행되어 버려서

위 코드의 두 가지 경우의 콘솔을 모두 찍는다.



실제의 경우 단순히 콘솔을 찍는게 아닌, 서버에 데이터를 요청하는 것이 되어버리므로 뒤로가기나 앞으로가기를 연속해서 누르는 경우만 캐시의 가치를 가지고, 그게 아닌 경우는 서버에 부하를 주게 된다.



이 문제를 해결하기 위해서 리덕스에 저장하는 값을 두가지가 아닌 세가지로 바꾸었다.



#### 네 번째 아이디어



리덕스 noCache 의 값을 단순 true, false 가 아닌 세 가지 값으로 나누기.

0 -> 기본 값

1 -> 캐시

2 -> 캐시 없이

**A page** 처음 상태는 0

빠져 나올 때, history의 action에 따라 구분, 예를 들어 링크로 이동했을 시 2로 설정.

**B page** 에서 useEffect 안에서 리덕스 noCache 값이 true면 (0 이 아니면), 그때 1 or 2 인지 구별하여 데이터 가져오기.

가져온 후에는 꼭 기본값인 0으로 리덕스 값 변경해주기.



이러면 B page의  useEffect()가 링크이동 때 리덕스가 변경되는 과정을 목격(?) 하더라도

0 -> 1 을 목격하거나

0 -> 2 를 목격한다.

아까  세 번째 아이디어에서는 1(true) -> 2(false) 를 목격해버려서 실제 데이터도 fetch하고 캐시값을 가져왔다면,

이번에는 아무 효력 없는 0을 먼저 목격한 후 바뀌는 것을 감지하므로, 실제 데이터와 캐시값을 두번이나 가져오는 불상사는 발생하지 않는다.












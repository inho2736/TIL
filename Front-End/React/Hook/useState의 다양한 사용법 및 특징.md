# useState 의 다양한 사용예시 , setState 특징

>useState 의 state값으로 어떤것들이 들어갈 수 있고, 그에따른 갱신방법을 다룬다.
>
>또, setState 의 비동기적 특징을 살펴본다.



useState는 useEffect와 함께 리액트 훅의 기본의 기본이다.



```javascript
import React, { useState } from "react";
```

이렇게 import 하고



```javascript
const [state, setState] = useState();
```

요렇게 사용한다.

state 와 setState의 이름은 자유자재로 바꿀 수 있다.



hook 폴더에 따로 함수만 넣고 다른 컴포넌트에서 import 해서 쓰는 경우가 많다. 

그래서 보통 useState를 사용한  hook의 return값으로 state와 setState를 반환한다.

```javascript
return [state, setState];
```



아직 초보라 대략 이렇게 쓴다는 것만 알고있다. (나중엔 좀더 유연하게 사용하겠지,,)

컴포넌트에서는 위의 hook을 import 한 후

```javascript
const [state, setState] = useTodo(); //useTodo()는 훅 이름
```

이렇게 리턴값을 이용해 사용한다. 



hook에서 state는 다양한 타입이 될 수 있다.



1. boolean

```html
import React, { useState } from 'react';
import ReactDOM from 'react-dom';

function LessText({ text, maxLength }) {
    const [hidden, setHidden] = useState(true); //여기주목!
    if (text <= maxLength) {
      return <span>{text}</span>
    }
    return (
         <span>
           {hidden ? `${text.substr(0, maxLength).trim()} ...` : text}
           {hidden ? (
             <a onClick={() => setHidden(false)}> read more</a>
           ) : (
             <a onClick={() => setHidden(true)}> read less</a>
           )}
         </span>
    );
}
```



2. number

```html
function StepTracker() {
  	const [steps, setSteps] = useState(0); //여기주목!
    function increment() {
      	setSteps(steps => steps + 1);
    }
    return (
        <div>
          Today you've taken {steps} steps!
          <br />
          <button onClick={increment}>I took another step</button>
        </div>
    );
}
```



3. array

```html
function RandomList() {
  	const [items, setItems] = useState([]);
    const addItem = () => {
        setItems([ //배열표시 있음에 유의
          ...items,
          {
            id: items.length,
            value: Math.random() * 100
          }
        ]);
     };
    return (
        <>
          <button onClick={addItem}>Add a number</button>
          <ul>
            {items.map(item => (
              <li key={item.id}>{item.value}</li>
            ))}
          </ul>
        </>
      );
}
```

 array를 state로 쓰면 보통 setState로는 array에 새 요소를 넣게된다. 이때 기존꺼랑 합치거나 이런 복잡한 코드 없이

ES6의 spread 연산으로 기존 state 배열에 새 객체를 요소로 추가할 수 있다. 이때 배열 괄호에 유의하기.



4. state with multiple keys (useReduser를 이용하면 효과적이라던데 요건 다음에 자세히 보기로,,)

``` html
function LoginForm() {
    const [form, setValues] = useState({
      username: '',
      password: ''
    });
    const printValues = e => {
        e.preventDefault();
        console.log(form.username, form.password);
      };
    const updateField = e => {
        setValues({ 
          ...form,
          [e.target.name]: e.target.value
        });
      };
    return (
        <form onSubmit={printValues}>
          <label>
            Username:
            <input
              value={form.username}
              name="username"
              onChange={updateField}
            />
          </label>
          <br />
          <label>
            Password:
            <input
              value={form.password}
              name="password"
              type="password"
              onChange={updateField}
            />
          </label>
          <br />
          <button>Submit</button>
        </form>
      );
}
```



출처: [https://medium.com/@shlee1353/%EB%A6%AC%EC%95%A1%ED%8A%B8-hooks-usestate-4%EA%B0%80%EC%A7%80-%EC%83%81%EC%9A%A9%EB%B0%A9%EB%B2%95-dfe8b2096750](https://medium.com/@shlee1353/리액트-hooks-usestate-4가지-상용방법-dfe8b2096750)



이렇게 여러 타입을 state로 쓸 수 있다. 



### setState의 비동기성



요번에 간단한 투두리스트를 만들면서 state로 array를 쓰게 되었다.

그리고 간간히 setState 이후의 state를 확인하고싶어서, setState 바로 다음에 

```javascript
setState(...);
console.log(state);
```

를 넣었는데, 자꾸 setState 가 실행되기 전의 state상태가 출력되었다.

아니 분명 바로 위에서 갱신했는데 왜이러지,, 했는데 setState는 비동기로 state를 업데이트 한다는 것을 알게되었다.

setState이 요청만 되고 바로 콘솔에 찍는 것이라, setState가 완료되었다는 보장이 없는 것이다.



그래서 함수형 setState를 사용할 수 있는데, 콜백함수를 넣으면 그 함수가 실행된 후 리턴값으로 state를 갱신한다.

이때 콜백함수의 인자는 바로 직전의 state이다.



예를 들어 

```javascript
this.setState({num: num + 1});
this.setState({num: num + 1});
this.setState({num: num + 7});
```

이렇게 하면 state 의 num은 마지막에 갱신된 7이 된다. 앞 두줄은 하나마나인 결과가 되는거다.



대신 

```javascript
this.setState((prevState) => ({num: prevState.num + 1}));    
this.setState((prevState) => ({num: prevState.num + 1}));    
this.setState((prevState) => ({num: prevState.num + 1}));
```

이렇게 하면 셋다 유의미!, 이전값을 보장하니 최종으로 3이 더해진다.



나는 

``` javascript
setTodoList(list => {
    const newList = 
    [
    	...list, 
    	{ 
    		id: list.length, 
    		todo 
    	}
    ];
    console.log(newList);
    return newList;
});
```

이렇게 리스트의 최신상태를 콘솔에 출력하고 갱신했다.



아까 다양한 타입을 사용하는 state 2번 예제에서 본 함수도 이것을 이용한 것이다.

```javascript
function increment() {
    setSteps(steps => steps + 1);
}
```



다르게 사용하는 예제도 있던데 다음기회에 더 자세히 알아보겠다.



### setState 쓰면 자주 나오는 Error

Error: Maximum update depth exceeded. This can happen when a component repeatedly calls setState inside componentWillUpdate or componentDidUpdate. React limits the number of nested updates to prevent **infinite loops**.

 

인피니트 루프를 보면 잠시 이성을 상실할 수 있으나,,

보통 이 문제는 

```html
<Button outline color="danger" onClick={deleteList(key)}>
	Delete
</Button>
```

요런 부분때문에 생긴다. 

onClick에는 콜백 함수를 넣어줘야하지 함수를 호출해버리면 안된다.



만약 위와 같이 함수를 호출해 버리면(특히 setState처럼 렌더링을 부르는 함수)

렌더링 되면서 저 문법때문에 함수 호출 -> 함수 내부에서 setState 작동으로 다시 렌더링 ->

그 렌더링으로 또 저 문법때문에 함수 호출,,,,,

대환장 파티가 된다.



일반 함수의 경우 그냥 함수명만 쓰면 콜백함수니까, 인자가 있는 함수를 쓸 때 아무렇지 않게 저렇게 호출해버려서 나오는 게 대부분이다.

```html
<Button
    outline color="danger"
    onClick={() => {           
            deleteList(num);
        }
    }>    
    Delete
</Button>
```

그땐 꼭 정신차리고 이렇게 쓰자. 





참고한 문서 : https://jaeyeophan.github.io/2018/01/02/React-tips-for-beginners/

[https://medium.com/wasd/setstate-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0-28b207fc81df](https://medium.com/wasd/setstate-파헤치기-28b207fc81df)


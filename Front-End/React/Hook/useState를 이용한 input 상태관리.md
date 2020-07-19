# useState를 이용한 input 상태관리

>input 의 value를 state와 연결하여 관리하는 방법에 대해 다룬다.



### 클래스형 컴포넌트



먼저 클래스형 컴포넌트에서 state로 input을 다루는 기본적인 모습을 살펴보면

jsx 는

```jsx
 <form onSubmit={this.handleSubmit}>
        <input
          placeholder="이름"
          value={this.state.name}
          onChange={this.handleChange}
          name="name"
        />
        <input
          placeholder="전화번호"
          value={this.state.phone}
          onChange={this.handleChange}
          name="phone"
        />
        <button type="submit">등록</button>
</form>
```

이런 form태그를 사용한다. input태그의 value속성에 state를 연결하고, onChange로 핸들러를 연결한다.

핸들러는 아래와 같이 사용한다.

```jsx
state = {
    name: '',
    phone: ''
}  
handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value
    })
}
```

입력하는 내용이 바뀔때 마다 핸들러가 작동하여 state도 동시에 변화시켜준다.

이름과 전화번호 두 input태그의 onChange 핸들러를 함께 쓰려면 각 input의 name 속성을 지정한 후

```jsx
 this.setState({
      [e.target.name]: e.target.value
    })
```

이렇게 e.target.name을  key값으로 해 state에 접근할 수 있다. 

( 변수를 key값으로 사용할 때는 위 예시처럼 대괄호( [ ] )로 감싸주어야 한다.)



전체적인 코드를 보면 다음과 같다.

```jsx
import React, { Component } from 'react';

class PhoneForm extends Component {
  state = {
    name: '',
    phone: ''
  }
  handleChange = (e) => {
    this.setState({
      [e.target.name]: e.target.value
    })
  }
  handleSubmit = (e) => {
    // 페이지 새로고침과 url변경 방지
    e.preventDefault();
    // 상태값을 onCreate 를 통하여 부모에게 전달 (onCreate는 부모에서 자식으로 전달된 props이다.)
    this.props.onCreate(this.state);
    // 상태 초기화
    this.setState({
      name: '',
      phone: ''
    })
  }
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          placeholder="이름"
          value={this.state.name}
          onChange={this.handleChange}
          name="name"
        />
        <input
          placeholder="전화번호"
          value={this.state.phone}
          onChange={this.handleChange}
          name="phone"
        />
        <button type="submit">등록</button>
      </form>
    );
  }
}

export default PhoneForm;
```



### 함수형 컴포넌트(Hooks)

hooks를 사용하는 함수형 컴포넌트의 경우 아주 살짝 달라진다.



useState로 초기 state와 setState를 가져오고

```jsx
const [ state, setState ] = useState ({
    name: '',
    phone: ''
  })
```



클래스형 컴포넌트를 함수형 컴포넌트로 바꾸며, 코드 구석구석에 있는 this를 없앤다,, ( 함수형 컴포넌트 쓸때 가장 쾌감을 느끼는 부분이다 ). props(이 예제에서는 onCreate 함수)도 파라미터에서 구조분해 할당으로 받아온다.

그리고 클래스형 setState와 달리 useState의 setState를 사용할 때는 spread 연산자 (...)를 반드시 사용해야 한다.



```jsx
 handleChange = (e) => {
    setState({
        // 추가된 spread 연산자
        ...state,
      [e.target.name]: e.target.value
    })
  }
```



클래스형 컴포넌트의 setState는 제시한 key값만 찾아 value를 바꾸지만, useState의 setState는 그냥 state를 통째로 교환하는 느낌이다. 

따라서 기존 state값을 spread연산으로 주고, 이후에 name과 일치하는 key값을 위에 덮어서 수정하는 식으로 작성해야 한다.

전체 코드를 보면 다음과 같다.

```jsx
import React, {useState} from 'react';

const PhoneForm = ({ onCreate }) => {
  
  handleChange = (e) => {
    setState({
        ...state,
      [e.target.name]: e.target.value
    })
  }
  handleSubmit = (e) => {
    // 페이지 새로고침과 url변경 방지
    e.preventDefault();
    // 상태값을 onCreate 를 통하여 부모에게 전달 (onCreate는 부모에서 자식으로 전달된 props이다.)
    onCreate(state);
    // 상태 초기화
    setState({
      name: '',
      phone: ''
    })
  }
 
  return (
      <form onSubmit={handleSubmit}>
          <input
              placeholder="이름"
              value={state.name}
              onChange={handleChange}
              name="name"
              />
          <input
              placeholder="전화번호"
              value={state.phone}
              onChange={handleChange}
              name="phone"
              />
          <button type="submit">등록</button>
      </form>
  );

}

export default PhoneForm;
```


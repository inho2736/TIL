# useEffect의 기초

>이글루 프로젝트를 하면서 훅을 여기저기 남발(?) 했다. 쓰면서 구조가 꼬여서 잠시 멈춰 생각해보니 useEffect에 대해서는 따로 기초를 정리해본 기억이 없었다. 그래서 벨로퍼트님과 리액트 공식문서를 보며 기본 개념을 다져보려 한다.



useEffect 는 함수 컴포넌트에서 side effect(데이터 가져오기, 수동으로 DOM수정하기 등)를 수행할 수 있는 hook이다.

클래스 컴포넌트에서 사용하는 componentDidMount, componentDidUpdate, componentWillUnmount 가 짬뽕된 것이라고 생각하면 된다.

#### useEffect의 형태

크게 세가지 형태가 있다.

1.  **빈 deps를 가진 useEffect**

   

   ```javascript
     useEffect(() => {
       console.log('컴포넌트가 화면에 나타남');    
     }, []);
   ```

   이 경우는 컴포넌트가 마운트 될 때, 앞의 함수가 수행된다. 

   여기에 앞 함수에 return 을 넣으면 언마운트 될 때 return 된 함수가 수행된다.

   ```javascript
    useEffect(() => {
       console.log('1. 마운트');
       return () => {
           console.log('1. 언마운트')
       }
     }, []);
   ```

   딱 해당 컴포넌트가 마운트 될 때 와 언마운트 될 때만 정직하게 나와서 가장 단순하다.





2.  **deps가 없는 useEffect**

   

   ```javascript
    useEffect(() => {
       console.log('컴포넌트가 화면에 나타남');    
     });
   ```

   이 경우는 컴포넌트가 리렌더링 될때마다 수행된다.

   마운트는 기본 ,,,내부 state가 변화 할 때마다 매번~ 수행된다.

   ```javascript
    useEffect(() => {
       console.log('1111'); 
       return () => {
           console.log('2222')
       }
     });
   ```

   이렇게 쓰면 뭘 하더라도 무한으로 생성되는 콘솔로그들을 볼 수 있다.



​		특히 부모컴포넌트 안에서   useState를 쓰고 그 자식 컴포넌트에서 저 useEffect를 쓰면, (부모가 리렌더링 하면 자식도 리렌더링 하므로) 부모 값이 바뀔 때 + 자식 값이 바뀔 때 가 혼합되어 난리가 날 수 있다.



3. **deps 안에 특정 값이 있는 useEffect**

   

   ```javascript
   useEffect(() => {
       console.log('컴포넌트가 화면에 나타남');    
     }, [foo]);
   ```

   이 경우는 **마운트 될 때**와 **foo의 값이 변경될 때**마다 앞의 함수가 수행된다.

   여기에 return 함수를 넣으면 값이 변경되기 직전에 return의 콜백함수가 수행된다.

   

   ```javascript
   useEffect(() => {
       console.log('1.마운트');   
       return () => {
           console.log('2. 언마운트')
       }
     }, [foo]);
   ```

   이렇게 찍으면 처음 마운트 될 때 1번이 수행된다.

   그 다음 foo값이 변경되면 먼저 2가 수행되고 그 다음 1이 수행된다.

   (그냥 값이 바뀌면 컴포넌트가 언마운트 되었다가 다시 마운트 된다고 생각하면 쉽다.)

   마지막으로 진짜 언마운트 될 때 2가 수행된다.

   

   


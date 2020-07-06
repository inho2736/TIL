# React 에서 부트스트랩을 이용하기

>리액트를 쓰기 전 부트스트랩을 간단하게 사용한 적 있었다. 컴포넌트를 불러와서 쓰기도 하고 그냥 부트스트랩 테마를 통째로(...) 갖다 쓰기도 했다.
>
>리액트를 접하고 나서 컴포넌트를 갖다 쓸 때는 아는 선배가 쓰던 material UI를 주로 썼다. 깔끔하긴 한데 화면을 꾸밀 요소는 약간 부족한가.. 하고 느꼈을 때 리액트에서도 부트스트랩을 쓸 수 있다는 것을 알게됐다.



일단 패키지 설치

```
npm install --save bootstrap
npm install --save reactstrap react react-dom
```



그다음 부트스트랩 css파일 import

```
import 'bootstrap/dist/css/bootstrap.css';
```



사용할 때는 리액트스트랩에서 필요한 컴포넌트의 이름을 import해 사용한다.

예를들어

```
import { Button } from 'reactstrap';
```

이렇게



다른 다양한 컴포넌트들을 보려면 

https://reactstrap.github.io/components/alerts/ 이 문서를 참고하자!
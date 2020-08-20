# Callback Hell 

>드림코딩 엘리 영상과 벨로퍼트님의 모던 자바스크립트 git book을 참고해 작성한 글이다. 

>Promise가 나오기 전, 비동기 처리는 거의 콜백만을 이용하여 처리하였다고 한다. 콜백 지옥이란 무엇인지, 그리고 어떤 문제점이 있는지 알아보자.



자바스크립트에서 서버와의 통신은 비동기로 처리해야, 불필요한 대기시간을 줄일 수 있다.

또한 서버 통신이 완료될 때 까지 빈 화면만 주구장창 쳐다보아야 하는 불상사를 막을 수 있다.



**콜백 지옥**이란 이런 비동기 처리가 겹겹이 쌓여, 콜백 안에서 또 콜백을 넣고 그 콜백 안에 다른 콜백을 넣어 코드의 depth가 미친듯이 증가하는 현상을 말한다.



#### 콜백 지옥 example



서버단 코드

```javascript
class UserStorage {
  loginUser(id, password, onSuccess, onError) {
    setTimeout(() => {
      if (
        (id === "inho" && password === "dream") ||
        (id === "coder" && password === "academy")
      ) {
        onSuccess(id);
      } else {
        onError(new Error("not found"));
      }
    }, 2000);
  }
  // 원래는 로그인 하면서 역할 한번에 받아와야 함
  // 거지같이 짠 백엔드라고 가정해보자
  getRoles(user, onSuccess, onError) {
    setTimeout(() => {
      if (user === "inho") {
        onSuccess({ name: "inho", role: "admin" });
      } else {
        onError(new Error("no access"));
      }
    }, 1000);
  }
}
```



콜백 지옥

```javascript
const UserStorage = new UserStorage();
const id = prompt("enter your id");
const password = prompt("enter your password");
UserStorage.loginUser(
  id,
  password,
  user => {
    UserStorage.getRoles(
      user,
      userWithRole => {
        alert(
          `Hello ${userWithRole.name}, you have a ${userWithRole.role} role`
        );
      },
      error => {
        console.log(error);
      }
    );
  },
  error => {
    console.log(error);
  }
);
```

먼저 로그인 할 정보와 함께 로그인에 성공, 실패 했을 때 호출할 함수를 콜백으로 전달한다.

로그인 성공 시 getRoles로 역할을 받아와야 하므로  콜백 안에서 getRoles를 수행하고

역할을 성공적으로 받아왔을 시 user의 이름과 역할을 알림창에 띄우는 함수를 콜백으로 전달한다.



지금은 두단계 정도밖에 깊어지지 않았지만, 실제로 서버에서 이런 작업을 할 시 더 복잡해질 가능성이 있다.



콜백지옥 레전드 사진...

![img](https://i.imgur.com/FDzn5s0.png)



#### 콜백 지옥의 문제점은 다음과 같다.



1. 읽기가 거북하고 가독성이 떨어진다. 따라서 비즈니스 로직을  찾기 어렵게 만든다.

2. 에러 발생 시 디버깅이 어렵다. 어디서 에러가 났는지 한눈에 파악할 수 없다.

3.  유지 보수가 힘들다.
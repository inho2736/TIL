# CSS Basic & Selector

>생활코딩 css 강의를 빠르게 훑고, 나중에 다시 궁금할 때 찾아볼 수 있도록 키워드를 정리하는 문서이다.
>
>https://www.opentutorials.org/course/2418



#### 1. css 소개 (정보와 디자인의 분리)



html 에도 디자인 태그는 있음 (ex. font)

정보 전달적인 태그와 디자인 적인 태그가 혼용되어 있으면 정보 전달의 기능을 제대로 하지 못함. 코드 복잡;



그래서 html은 정보 전달 기능을, css는 디자인 기능을 할수 있게 분리. (style 태그 사용)



```html
<style>
    li{
        color:red;
    }
</style>

<li>dfsd</li>
<li>sfsd</li>
<li>asger</li>
```



+a 모든 li의 디자인을 한번에 바꿀수 있으므로 html(모두 태그를 씌워줘야 하는)에 비해 효율적



#### 2. html 과 css 가 만나는법



- 인라인 방식

  ```html
   <h1 style="color:red">Hello World</h1>
  ```

- style 태그 (이 안에는 css문법이 들어간다.)

  ```html
   <style>
        h2{color:blue}
   </style>
  ```

  

### 선택자



#### 1. 선택자와 선언

1. 선택하고 (선택자) (li)
2. 선택한 대상에게 효과를 줘야 한다. (선언) ({
           color (property(속성)):red (value); // 
           text-decoration:underline;
         })

```html
<html>
  <head>
    <style>
      li{
        color:red;
        text-decoration:underline;
      }
    </style>
  </head>
  <body>
    <ul>
      <li>HTML</li>
      <li>CSS</li>
      <li>JavaScript</li>
    </ul>
  </body>
</html>
```



#### 2. 선택자의 종류



- 태그 선택자 (앞의 예제)

- 아이디 선택자

  ```html
  <style>      
        #select{
          font-size:50px;
        }
  </style>
  
  <li>HTML</li>
  <li id="select">CSS</li>
  <li>JavaScript</li>
  ```

  

- 클래스 선택자

  ```html
  <style>      
        .deactive{
          text-decoration: line-through;
        }
  </style>
  
  <li class="deactive">HTML</li>
  <li id="select">CSS</li>
  <li class="deactive">JavaScript</li>
  ```

  어떤 대상을 관리하기 쉽게 grouping 하는 것을 class라고 함. id는 한개 한개 구분하는 느낌(학번처럼). 

  즉 id는 한번만 사용하고, 여러개 등장하면 안됨. 여러번 쓸때는 class 로 바꿔야 바람직



#### 3. 부모 자식 선택자



어떤 태그의 하위에 있는 태그를 선택하고 싶을 때는 좀 더 복합적인 선택자를 사용



중간에 띄어쓰기 하면, ul 밑의 li 에만 스타일 적용

(조상이 있으면 자손 전부)

```html
<style> 
     ul li{
        color:red;
  	}
</style>	
 
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
<ol>
    <li>HTML</li>
    <li>CSS
</ol>

```



#lecture>li 는 id lecture의 **직계자손** li만 스타일 적용

(부모와 자식)

```html
<style> 
    #lecture>li{
        border:1px solid red;
    }
</style>	

<ol id="lecture">
    <li>HTML</li>
    <li>CSS
        <ol>
            <li>selector</li>
            <li>declaration</li>
        </ol>
    </li>
    <li>JavaScript</li>
</ol>

```



ul,ol 이렇게 하면 ul과 ol 에 모두 스타일 적용

```html
<style>

    ul,ol{
   		background-color: powderblue;
    }
</style>
```



#### 4. 선택자 공부 팁



CSS 선택자를 게임의 형식을 통해서 익힐 수 있는 사이트

http://flukeout.github.io/



각종 필요한 css정보를 간략히 정리한 컨닝페이퍼

Css Cheat Sheet (구글 이미지 검색)



#### 5.  가상 클래스 선택자



가상(pseudo) 클래스 선택자는 클래스 선택자는 아니지만 엘리먼트들의 상태에 따라서 마치 클래스 선택자처럼 여러 엘리먼트를 선택할 수 있다는 점에서 붙은 이름. 



```html
<style>
    
    
    a:hover{
        color:yellow;
    }
    a:active{
        color:green;
    }
    a:focus{
        color:white;
    }
    
</style>
<a href="https://a.a">방문안함</a>
<a href="https://b.b">방문함</a>
```

hover를 active 아래에 쓰면, 클릭해도 마우스가 위에 있으므로, 뒤의 호버가 덮어써져서 액티브가 안먹음. 꼭 호버를 앞에쓰자

포커스도 비슷한 이유로 뒤에써라



###### 링크와 관련된 가상 클래스 선택자

- :link - 방문한 적이 없는 링크

- :visited - 방문한 적이 있는 링크

  (visited는 보안상 이유로 일부 속성 제한)

- :hover - 마우스를 롤오버 했을 때 

- :active - 마우스를 클릭했을 때 



**위의 선택자는 순서대로 지정하는 것이 좋다**

특히 visited의 경우는 보안상의 이유로 아래와 같은 속성만 변경이 가능. 

- color
- background-color
- border-color
- outline-color
- The color parts of the fill and stroke properties



#### 6. 여러가지 선택자들

​	나중에 필요할때 찾아듣기

​	https://www.opentutorials.org/course/2418/13583






# CSS Property

>https://www.opentutorials.org/course/2418
>
>생활코딩 CSS강의의 속성 파트



####  속성을 공부하는 방법



CSS의 효과 다른 말로 속성(property)는 약 250개 정도. 일상 생활에서 사용하는 자연어의 어휘에 비하면 몇개 되지 않지만, 이것들을 하나씩 열거해서 공부하는 것은 뇌를 혹사시는 일. 

방법은 가장 빈도수가 높은 CSS의 속성 순으로 공부하고, 빈도가 낮은 것들은 그런 것이 있다는 정도만 확인하고 필요할 때 검색, 질문 등을 이용하는 것.



속성 빈도수 통계 (microsoft)

https://developer.microsoft.com/en-us/microsoft-edge/platform/usage/



### 타이포그래피



#### 1. font-size

폰트 크기는 단위가 중요함. 주요 단위로는 px, em, rem이 있다.

###### rem

html 태그에 적용된 font-size의 영향을 받습니다. html 태그의 폰트 크기에 따라서 상대적으로 크기가 결정되기 때문에 이해하기 쉽습니다. 가장 바람직한 단위입니다. ***이것을 사용하세요*.** 

###### px

모니터 상의 화소 하나의 크기에 대응되는 단위입니다. 고정된 값이기 때문에 이해하기 쉽습니다만, 사용자가 글꼴의 크기를 조정할 수 없기 때문에 가급적 사용을 하지 않는 것이 좋습니다. 

###### em

부모 태그의 영향을 받는 상대적인 크기입니다. 부모의 크기에 영향을 받기 때문에 파악하기가 어렵습니다. rem이 등장하면서 이 단위 역시 사용이 권장되지 않습니다. 



사용자가 브라우저 글꼴 사이즈 바꿨을 때(줌인 말고) **px**는 안바뀌고 **rem**은 바뀜



```html
<style>
    #px{font-size:16px;}
    #rem{font-size:1rem;}
</style>
<body>
    <div id="px">PX</div>
    <div id="rem">REM</div>
</body>
```



#### 2. color

3 가지 방식

- name
- rgb color
- hexadecimal color

```html
<style>
    h1{color:#00FF00;}
</style>
```



#### 3. text-align

text-align의 값으로 올 수 있는 값은 아래와 같다. 

- left
- right
- center
- justify (텍스트 사이의 간격을 조절해 왼쪽 오른쪽 정렬)

```html
<style>
    p{
        text-align: justify;
        border:1px solid gray;
    }
</style>

<p>

    Integer commodo varius ornare. Vivamus lacus urna, scelerisque nec lectus porta, interdum commodo dolor. Curabitur sagittis diam quis tellus semper commodo. Ut non orci consectetur, cursus urna et, tincidunt est. Donec mollis vulputate tempus. Aliquam sapien leo, venenatis at ligula vitae, vestibulum finibus ipsum. Donec pulvinar pretium mattis. Mauris risus augue, eleifend et suscipit ac, convallis vel nisl. Fusce tincidunt fringilla vulputate. Ut porttitor lorem vitae sodales finibus.
</p>
```



#### 4. font

#### 5. 서체

#### 6. 웹폰트

​	later,,



### 조화



>HTML 은 엘리먼트끼리 중첩된 구조를 가진다. 그래서 한 엘리먼트는 다양한 요소의 영향을 받게 된다. 
>
>여러 효과가 한 엘리먼트를 두고 대치하고 있을 때, 브라우저가 어떤 효과의 손을 들어줘야 하는가에 대한 결정을 해야 한다. 
>
>이를 위한 여러 규칙들을 살펴본다.



#### 1 . 상속



상속은 부모 엘리먼트의 속성을 자식 엘리먼트가 물려받는 것

상속은 CSS에서 생산성을 높이기 위한 중요한 기능.

가령 여러 엘리먼트에 일일히 속성을 넣는거보다

부모 엘리먼트에 한번에 속성을 주는 것이 훨씬 효율적.



하지만 모든 속성이 상속되는 것은 아님. 상속을 하면 오히려 비효율적인 속성도 존재. 

상속하는 속성과 상속하지 않는 속성의 목록 

https://www.w3.org/TR/CSS21/propidx.html



#### 2. stylish



stylish는 웹사이트의 디자인을 사용자 마음대로 수정할 수 있는 기능입니다. 이 기능을 이용해서 주어진데로 웹페이지를 소비하는 것이 아니라 자신의 취향을 반영할 수 있습니다. 또 다른 사람이 만든 디자인을 적용해서 간편하게 사이트의 디자인을 변경할 수도 있습니다. 

https://userstyles.org/



#### 3. Cascadeing



CSS는 Cascading Style Sheet의 약자

캐스케이딩은 폭포 라는 의미

웹페이지의 디자인이 **웹브라우저**의 기본 디자인과 브라우저 **사용자**의 디자인 그리고 웹페이지 **저자**의 디자인이 결합될 수 있다는 점에 착안

이렇게 다양한 효과가 영향력을 행사하려고 할 때는 우선순위가 필요함.

기본적인 우선순위는 **웹브라우저 < 사용자 < 저자**



```html
<style>
    li{color:red;}
    #idsel{color:blue;}
    #idsel{color:yellow;}
    .classsel{color:green;}
</style>
<ul>
    <li>html</li>
    <li id="idsel" class="classsel">css</li>
    <li>javascript</li>
</ul>
```

style attribute (inline 방식)이 1순위

그 다음 id, class, tag 선택자

포괄적인것 보다, 구체적이고 명시적인 것 순서로 우선순위를 가진다고 기억하면 쉽다.



모든 우선순위를 다 뛰어넘을 수 있는 방법이 있다. 아래와 같다.

좋은 방법은 아님,,

```css
li{color:red; !important;}
```



### 레이아웃



>정보를 잘 정리 정돈해서 일관된 모습으로 보여지도록 하는 것은 디자인에서 매우 중요한 주제이다. 
>
>구획을 나누고 적절히 정보를 배치하는 것을 레이아웃(layout)이라고 한다. 안타깝게도 CSS를 이용해서 레이아웃을 잡는 것은 다소 어려운 것이 사실이다. 



#### 1. 인라인 vs 블럭레벨



html 엘리먼트들은 크게 두가지로 구분

- 화면 전체를 사용하는 태그 => block level element
- 화면의 일부를 차지하는 태그 => inline level element



```html
<style>
    h1,a{border:1px solid red;}   
</style>
<body>
    <h1>Hello world</h1>
    안녕하세요. <a href="https://opentutorials.org">생활코딩</a>입니다.
</body>
```



h1 태그는 화면 가로 전체를 차지하는 block level element

그래서 다음에 뭘 쓰면 자동을 다음줄에 나타남



반면  a 태그는 개행 안되는 inline level element



어떤 태그가 어떤 레벨 엘리먼트인지는 각자 효율적인 방법으로 지정되어 있음, h1 같은 제목은 블럭레벨인게 효율적이고, a 같은 링크는 개행 안되는 인라인 레벨인게 효율적.



대신 임의로 바꿀 수 있음.

```css
h1{display: inline;}
a{display: block;}
```



#### 2. 박스모델

>중요!



```html
<style>
    p{
        border:10px solid red;
        padding:20px;
        margin:40px;
        width: 120px;        
    } 
</style>

<p>
	lskdjf;lskdjf;lksdj;glkjkdfbn,mwfok
</p>
<p>
	lskdjf;lskdjf;lksdj;glkjkdfbn,mwfok
</p>

```

padding 은 상자 내부 contents와 테두리(border)와의 간격

margin 은 상자끼리의 간격

inline level element에서는 width와 height 값은 무시됨. 



#### 3. box-sizing

box-sizing은 박스의 크기를 화면에 표시하는 방식을 변경하는 속성이다.  

**width와 height는 엘리먼트의 컨텐츠의 크기를 지정**한다. 따라서 테두리가 있는 경우에는 테두리의 두께로 인해서 원하는 크기를 찾기가 어렵다. 

(같은 width와 height 에도 상자 크기가 달라질 수 있음)

box-sizing 속성을 border-box로 지정 (원래는 content-box)하면 테두리를 포함한 크기를 지정할 수 있기 때문에 예측하기가 더 쉽다. 최근엔 모든 엘리먼트에 이 값을 지정하는 경우가 늘고 있다. 



```html
<style>
    *{
        box-sizing:border-box;
    }
    div{
        margin:10px;
        width:150px;
    }
    #small{
        border:10px solid black;
    }
    #large{
        border:30px solid black;
    }
</style>

<div id="small">Hello</div>
<div id="large">Hello</div>

```



#### 4. 마진 겹침 현상





#### 5. 포지션

#### 6. flex

later~

#### 7. media query

later~

#### 8. float

#### 9. 다단 (muti column)

later~






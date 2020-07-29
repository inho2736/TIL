# Flex

>CSS의 flex는 엘리먼트들의 크기나 위치를 쉽게 잡아주는 도구이다. 지금까지 레이아웃과 관련된 다양한 속성들이 있었지만 그리 효과적이지 않았다.
>
>flex를 이용하면 레이아웃을 매우 효과적으로 표현할 수 있다.



table(표), position (static, relative, absolute), float 등등을 이용해 레이아웃을 구현하다가 갈팡질팡함.



그때 등장한 것이 Flex



### flex 기본



flex를 이용하려면 컨테이너와 아이템의 구조를 가져야한다.

```html
<container>
	<item></item>
    <item></item>
</container>
```



container 와 item에는 다른 속성들을 지정해주게 되어있음.



flex를 시작하려면 부모 엘리먼트(예시에서는 container)의 속성의 display에 flex를 주어야 한다.

flex의 정렬방법은 flex-direction으로 설정한다. 기본값은 row. 반대로 사용하고 싶다면 row-reverse를 사용한다.



flex item은 기본적으로 화면 전체를 쓴다.

```html
<style>
    .container{
        background-color: powderblue;
        height:200px;
        display:flex;
        flex-direction:row;
    }
    .item{
        background-color: tomato;
        color:white;
        border:1px solid white;
    }
</style>

<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
    <div class="item">5</div>
</div>
```



### grow & shrink



아이템은 컨테이너의 크기에 따라서 작아지기도 하고 커지기도 한다. 이 때 작아지고, 커지는 비율을 지정하는 방법이 바로 grow & shrink 이다.



flex-basis = 기본크기 설정

flex-grow = ( default = 0 ) => 꽉참기준에서 아이템이 차지하는 공간의 비율

flex-shrink =  ( default = 1 ) =>화면을 줄여서 컨테이너의 크기가 작아지면 공간을 가진(flex-basis나  grow로) 아이템이 공간을 줄이게 되는데, 공간 줄임에 대한 부담을 가지는 비율. 

0이면 컨테이너가 줄어들어도 아이템은 부담이 없어 줄어들지 않는다. 숫자를 가지면 그 숫자 비율만큼 빠르게 줄어든다.



```html
<style>
    .container{
        background-color: powderblue;
        height:200px;
        display:flex;
        flex-direction:row;
    }
    .item{
        background-color: tomato;
        color:white;
        border:1px solid white;         
    }
    .item:nth-child(1){
        flex-basis: 150px;
        flex-shrink: 1;
    }
    .item:nth-child(2){
        flex-basis: 150px;
        flex-shrink: 2;
    }
</style>

<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
    <div class="item">5</div>
</div>
```



### Holy grail layout (flex ver.)



이전 float 속성을 공부하며 holy grail layout을 float로 구현해 보았다. flex를 사용하면 이를 훨씬 간단하게 구현할 수 있다.



```html
<style>
    .container{
        display: flex;
        flex-direction: column;
    }
    header{
        border-bottom:1px solid gray;
        padding-left:20px;
    }
    footer{
        border-top:1px solid gray;
        padding:20px;
        text-align: center;
    }
    .content{
        display:flex;
    }
    .content nav{
        border-right:1px solid gray;
    }
    .content aside{
        border-left:1px solid gray;    
    }
    nav, aside{
        flex-basis: 150px;
        flex-shrink: 0;
    }
    main{
        padding:10px;
    }
</style>
<body>
    <div class="container">
        <header>
            <h1>생활코딩</h1>
        </header>
        <section class="content">
            <nav>
                <ul>
                    <li>html</li>
                    <li>css</li>
                    <li>javascript</li>
                </ul>
            </nav>
            <main>
                Lorem ipsum dolor sit amet, consectetur adipisicing elit. Reiciendis veniam totam labore ipsum, nesciunt temporibus repudiandae facilis earum, sunt autem illum quam dolore, quae optio nemo vero quidem animi tempore aliquam voluptas assumenda ipsa voluptates. Illum facere dolor eos, corporis nobis, accusamus velit, similique cum iste unde vero harum voluptatem molestias excepturi.
            </main>
            <aside>
                AD
            </aside>
        </section>
        <footer>
            <a href="https://opentutorials.org/course/1">홈페이지</a>
        </footer>
    </div>
</body>
```










# Bfs 와 Dfs 그리고 그래프의 기본

> 백준 1260번 Bfs 와 Dfs문제를 풀면서 작성한 글입니다.



그래프를 저장할 자료구조로 ArrayList의 배열을 선언했다.

```java
//리스트 배열 선언
ArrayList<Integer> [] list = new ArrayList[n + 1];

//각 배열 원소마다 리스트 생성
for(int i=1; i<=n; i++){
    list[i] = new ArrayList<Integer>();
}
```



a노드와 b노드가 연결된 edge를 표현할 때는

```java
for(int i=0; i<m; i++){
    list[a].add(b);
    list[b].add(a);
}
```

a 리스트에 b가 연결되었다는 표시로 원소를 넣고,

마찬가지로 b리스트에 a가 연결되었다는 표시로 원소를 추가한다.



이후 배열의 각 리스트들을 오름차순으로 정렬한다.

```java
for(int i=1; i<=n; i++){
	Collections.sort(list[i]);
}
```



## Dfs

깊이 우선 탐색



탐색 전에 방문한 노드인지 아닌지를 저장하는 boolean 배열을 선언한다.

```java
static boolean [] check = new boolean[n+1];
```



깊이우선 탐색은 주로 재귀나 스택을 이용해 이루어진다.

```java
public static void dfs(int x) {
    //방문 여부를 여기서 체크할 수도 있고................1
    if(check[x]) {
        return;
    }
    
    //방문 체크
    check[x] = true;
    
    //정답 리스트에 추가
    result.add(x);
    
    //x노드와 연결된 모든 노드를 y로 돌면서
    for(int y: list[x]) {
        
        //여기서 체크할 수도 있다.....................2
        if(!check[y]){
            
            //재귀
            dfs(y);
        }
    }
}
```



## Bfs

너비 우선 탐색



마찬가지로 탐색 전에 방문한 노드인지 아닌지를 저장하는 boolean 배열을 선언한다.

```java
static boolean [] check = new boolean[n+1];
```



너비우선 탐색은 주로 queue를 이용해 구현한다.

```java
public static void bfs(int x) {
    
    //큐 선언
    Queue <Integer> queue = new LinkedList<Integer>();
    
    //시작노드 추가
    queue.add(x);
    
    //방문여부 추가
    check[x] = true;

    //큐가 비었으면 중지
    while(!queue.isEmpty()) {
        
        //순서대로 한개를 비우고
        int i = queue.poll();
        
        //결과값에 저장
        result.add(i);
        
        //i에 연결된 j노드들을 for문을 돌면서 확인
        for(int j:list[i]) {
            
            //방문여부
            if(!check[j]) {
                check[j] = true;
                
                //큐에 추가
                queue.add(j);
            }
        }
    }
}
```


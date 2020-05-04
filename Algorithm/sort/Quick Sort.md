# Quick Sort

백준 

[11004]: https://www.acmicpc.net/problem/11004	"k번째 수"

 를 풀며 다시 정리한다.



## 설명

퀵 정렬은 분할정복(큰 문제를 작은 문제들로 나눠 해결하는 방법)이다.

간단하게 설명하자면



1. 리스트중 하나의 원소를 고른다. 이 원소는 **pivot** 이 된다.

2. pivot앞에는 pivot보다 작은 원소들이, pivot 뒤에는 큰 원소들이 위치하도록 리스트를 분할한다.
3. 분할된 두 리스트에서 ***재귀적으로*** 같은 과정을 반복한다.



재귀 호출이 한번 이루어질때마다 pivot 의 최종 위치가 정해지는 식이다. 



## 예제

여기서 빨간색이 pivot 으로 정해진 것

i 는 오른쪽으로 이동하며 pivot보다 큰 값을 찾고, 

j 는 왼쪽으로 이동하며 pivot보다 작은 (작거나 같은)값을 찾는다.

i<j 인 가정하에 두 위치의 값을 swap한다.

![퀵소트](https://t1.daumcdn.net/cfile/tistory/999E373A5ACB53AE07)



이렇게 이동하다가 어느 순간 i와 j가 만나는 지점이 온다.

그때의 위치가 바로 pivot이 위치해야 하는 지점이다.

![퀵소트](https://t1.daumcdn.net/cfile/tistory/99AD09415ACB54170D)



이렇게 한번 실행시 pivot 왼쪽에는 pivot보다 작은 원소들이 위치하고, 

 pivot오른쪽에는 pivot보다 큰 원소들이 위치하게 되어 ,결론적으로는 pivot의 최종 위치가 정해진다.



## 코드

```java
public class QuickSort {

	private static int[] arr = {5, 6, 2, 8, 7, 23, 4, 1};
	
	public static void main(String[] args) {
		
		quickSort(arr, 0, arr.length-1);
		
		for(int i=0; i<arr.length; i++) {
			System.out.println(arr[i]);
		}
	}
	private static void quickSort(int [] arr, int left, int right) {
		
		//left right 해서 원소가 한개이하면 return
		if (left >= right) {
			return;
		}
		
		//partition -> pivot의 위치를 정해줌
		int pivot = partition(arr, left, right);
		
		// 피봇 기준 왼쪽과 오른쪽 다시 재귀로 sort
		quickSort(arr, left, pivot-1);
		quickSort(arr, pivot+1, right);
	}
	private static int partition(int [] arr, int left, int right) {
		
		//지금은 pivot을 배열의 맨 앞으로 지정
		//배열이 역순으로 정렬되어있을 경우 매우 비효율적임 -> 이때는 pivot을 중간값으로
		int pivot = arr[left];
		int i = left;
		int j = right;
		
		//i 와 j 가 겹치지 않을때만 명령어 실행
		while(i < j) {
			
			//j는 오른쪽에서부터 pivot보다 작거나 같은 값을 찾아냄
			// pivot보다 크면 계속이동
			while(arr[j] > pivot){
				j--;
			}
			
			//i 는 왼쪽에서부터 pivot보다 큰 값을 찾아냄
			//pivot보다 작거나 같으면 계속 이동
			
			//바로 위 while문에서 j가 이동했을 때 (i < j) 조건이 깨졌을 수도 있으므로 한번 더 검사
			while((i < j) && arr[i] <= pivot) {
				i++;
			}
			
			//i랑 j 자리바꾸기
			swap(arr, i, j);
		}
		
		//i랑 j만나면 그자리를 pivot자리(현재는 제일 왼쪽) 와 바꿈
		swap(arr, i, left);
		
		//그 i자리가 피봇이 위치할 최종자리이므로 return
		return i;
	}
	private static void swap(int [] arr, int a, int b) {
		int tmp = arr[a];
		arr[a] = arr[b];
		arr[b] = tmp;
	}

}
```



## 시간복잡도

퀵소트의 평균 시간복잡도는 O(nlogn)

이 시간복잡도를 아주 간단하게 나타낸 그림이 있어 참고했다.

![img](https://gmlwjd9405.github.io/images/algorithm-quick-sort/sort-time-complexity-etc1.png)



최악 시간복잡도로 O(n^2) 이 나올 수 있다.

![img](https://gmlwjd9405.github.io/images/algorithm-quick-sort/sort-time-complexity-etc2.png)





#### 참고한 사이트 및 이미지 출처

https://mygumi.tistory.com/308

[https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC](https://ko.wikipedia.org/wiki/퀵_정렬)

https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html


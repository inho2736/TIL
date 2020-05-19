# Quick Selection 을 이용한 문제

> Boj11004 k번째 수
>
> quick sort를 활용한 quick selection문제이다.



## 문제

수 N개 A1, A2, ..., AN이 주어진다. A를 오름차순 정렬했을 때, 앞에서부터 K번째 있는 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N(1 ≤ N ≤ 5,000,000)과 K (1 ≤ K ≤ N)이 주어진다.

둘째에는 A1, A2, ..., AN이 주어진다. (-109 ≤ Ai ≤ 109)

## 출력

A를 정렬했을 때, 앞에서부터 K번째 있는 수를 출력한다.

## 예제 입력 1 복사

```
5 2
4 1 2 3 5
```

## 예제 출력 1 복사

```
2
```



## 풀이

정렬했을 때 특정 자리에 어떤 숫자가 들어가는 것을 모든 숫자들이 완벽하게 정렬되기 전에 알 수도 있다.

이전에 공부했던 퀵소트를 보면 

pivot이 하나 정해지고 그 pivot을 기준으로 왼쪽에 작은 값, 오른쪽에 큰 값이 위치하므로

다른 숫자들은 완벽하게 정렬되지는 않았지만 그 pivot은 최종적으로 어디에 위치할지를 알수있다.



따라서 위 문제도 퀵소트 방식으로 정렬하면서 찾되, pivot이 위치한 자리를 k와 비교하여

작다면 pivot의 왼쪽만 재귀를 통해 다시 소트하고, 

크다면 pivot의 오른쪽만 소트한다.

이 과정을 pivot의 위치와 k가 일치할때 까지 반복하면 된다.



그냥 통째로 퀵소트를 진행하는 것과는 다르게 

파티션이 나눠진 후 k번째가 있을 곳만 찾아서 정렬을 하므로 필요없는 곳은 정렬하지 않아 시간을 단축할 수 있다.



```java

import java.io.*;
import java.util.*;
public class Boj11004_K번째수 {
	static int result = -1;
	static int k = -1;
	public static void main(String[] args) throws IOException {
		
        // 입력받기
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		k = Integer.parseInt(st.nextToken());
		int [] arr = new int[n];
		
		st = new StringTokenizer(br.readLine());
		for(int i=0; i<n; i++) {
			arr[i] = Integer.parseInt(st.nextToken());
		}
        
        // 퀵 셀렉션 방식으로 찾음
		quickSort(arr, 0, n-1);
		for(int i=0; i<n; i++) {
			bw.write(arr[i] + " ");
		}
		bw.write(arr[k-1]+"");
		bw.flush();
		br.close();
		bw.close();
	}
	public static void quickSort(int [] arr, int left, int right) {
		if(left >= right) {
			return;
		}
		
        // 파티션을 나누고
		int pivot = partition(arr, left, right);
        
        // pivot의 자리보다 k자리가 더 작으면 pivot왼쪽만 다시 정렬
		if(pivot > k-1) {
			quickSort(arr, left, pivot-1);
		}
        
        // 반대의 경우 오른쪽만 다시 정렬
		else if(pivot < k-1) {
			quickSort(arr, pivot +1, right);
		}
        
        // k와 같은경우 반환 
		else {
			return;
		}
	}
	
	public static int partition(int [] arr, int left, int right) {
        
		// 파티션 방식은 일반 퀵소트와 같음
        // 여기선 pivot을 중간값으로 선택
		int mid = (left + right) / 2; 
	    swap(arr, left, mid);
		
		int pivot = arr[left];
		int i = left;
		int j = right;
		
		while(i<j) {
			while(arr[j] > pivot) {
				j--;
			}
			while(i < j && arr[i] <= pivot) {
				i++;
			}
			swap(arr, i, j);
		}
		swap(arr, i, left);
		return i;
	}
	
	public static void swap(int [] arr, int a, int b) {
		int tmp = arr[a];
		arr[a] = arr[b];
		arr[b] = tmp;
	}

}

```



> 독특한 점은 이 문제에서  pivot을 선정할 때 왼쪽값으로 정하는 방식을 썼더니 한 예제에서 시간초과가 났다.
>
> 백준의 입력값을 자세히 알 수는 없지만 만약 제시된 수들이 역순으로 있었을 경우 
>
> pivot을 가장 왼쪽값으로 설정하는 것이 시간 초과의 원인이 되었을 것이다.
>
> 그래서 중앙값을 선택하도록 코드를 살짝 수정하였다.
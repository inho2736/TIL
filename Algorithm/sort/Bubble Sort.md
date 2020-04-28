# Bubble Sort



![img](https://github.com/namjunemy/TIL/blob/master/Algorithm/img/sort_02.png?raw=true)

## 시간복잡도

( n - 1 ) + ( n - 2 ) +  ...  + 2 + 1   ---------> n ( n - 1 ) / 2 

**O(n^2)**



느린편이지만 구현이 간단해서 자주 쓰인다고 한다.



## Code

```java
public class BubbleSort {
	private static int[] arr = {5, 6, 2, 8, 7, 23, 4, 1};

	public static void main(String[] args) throws IOException{
		
		bubbleSort(arr, arr.length);
		for(int i=0; i<arr.length; i++) {
			System.out.println(arr[i]);
		}
	}
	
	private static void bubbleSort(int [] arr, int n) {
        //바꿔치기에 사용할 임시 변수
		int tmp;
        
        //위 그림보면 뒷자리부터 하나씩 완성되는 것을 바깥반복문으로(pass i)
		for(int i=0; i<n; i++) {
            
            //n-i-1 에 주목하기 (두개씩 비교하니 -1이 필요)
            //아니면 j를 1부터 시작해서 n-i 까지로 바꾸고
            //arr[j]와 arr[j-1]을 비교할 수도 있다.
			for(int j=0; j < n - i - 1; j++) {
                
                //뒤 숫자가 더 작으면 교환
				if(arr[j] > arr[j+1]) {
					tmp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = tmp;
				}
			}
		}
	}
}
```
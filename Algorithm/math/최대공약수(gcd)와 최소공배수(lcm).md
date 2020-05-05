# 최대공약수(gcd)와 최소공배수(lcm)

백준 2609 문제

빠르고 간단하게 구하기 위해서 유클리드 호제법을 사용한다.

```java
public static int GetGCD(int a, int b) {
		while(b > 0) {
			int tmp = a;
			a = b; // 1번 과정
			b = tmp % b; //2번 과정
		}
		return a; //while문을 나와 b가 0일때의 a가 gcd
	}
```



![image](https://user-images.githubusercontent.com/28773553/81062827-aa252b00-8f11-11ea-89a4-115c77554443.png)

lcm을 구하는 방법은 gcd를 이용하면 아주 간단해진다.



a * b / gcd 



a와 b를 곱하고 이 중 공약수라 두번 곱해진 gcd를 나눠주면 된다.




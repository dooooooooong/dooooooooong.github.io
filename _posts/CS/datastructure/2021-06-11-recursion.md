---
layout: post
class: post-template
navigation: true
title: "3. Recursion"
date: 2021-06-11 21:49:12
categories: CS/DataStructure
use_math: true
---
**재귀**(recursion)는 어떠한 것을 정의할 때 자기 자신을 참조하는 것을 뜻합니다. 순환정의 또는 **재귀**적 정의라고도 합니다.

컴퓨터 과학에서 재귀란 문제를 형태는 동일하지만 조금 더 작고 간단한 문제로 재정의하고 이를 반복하여 가장 간단하고 풀기쉬운 단계까지 문제를 단순화시키는 것을 말합니다.

<br>

 재귀에는 두 가지 단계가 있습니다. <br>

- 재귀 단계 (recursive case): 문제를 형태는 동일하지만 조금 더 작고 간단한 문제로 재정의하는 과정. 재귀 단계는 더 많은 수의 재귀 단계를 만들 수 있지만 단계가 심화될 수 록 이전 문제보다 조금 더 단순한 문제를 가지고 자기 자신을 호출합니다.
- 기저 단계 (base case): 재귀 단계에서 문제를 단순하게 할 때, 가장 단순하게 정의되는 케이스, 결국이 프로세스가 반복되고 문제의 크기가 각 단계에서 감소하면 매우 작고 해결하기 쉬운 문제에 도달합니다.



결국 재귀를 사용하여 문제를 해결할 때 핵심은 문제를 더 작지만 유사한 문제로 만드는 것입니다. 이 과정이 재귀에서 핵심 풀이법이며 여러가지 예제를 살펴보면서 원 문제를 더 작은 문제로 재정의하는 과정을 함께 진행해볼 것입니다. 



<br><br>

### 예시1.  Factorial

$ fact(n) = \cases { 1 & (n = 0) \cr n*fact(n-1) & (n > 0) } $



- Input: 0보다 크거나 같은 정수 n
- Output
  - n이 0일 때, 1
  - n이 0이 아닐 때, n x n-1 x n-2 x ... x 1



#### **재귀적 구조를 찾기 위해서**

- 어떻게 문제를 더 작지만 유사한 문제로 재정의할 수 있을까

: 가장 쉬운 방법은 가장 간단한 케이스부터 직접 손으로 써보고 한 단계씩 비교해보는 것입니다.

n = 1 인 경우 1 <br>n = 2 인 경우 2 * 1 <br>n = 3 인 경우 3 * 2 * 1 <br>.... <br>n번째 팩토리얼 함수는 n-1번째 팩토리얼 함수에 n을 곱한 패턴이 보입니다.

<br>

- 문제를 더 작게 재정의하는 방법을 찾았다면 그것의 기저단계는 무엇인가

: n이 0 혹은 1이 되면 1의 값을 가집니다.

<br>

위의 내용을 종합하여 코드로 옮기면 아래와 같습니다.

```c++
int factorial(int n) {
	if (n == 0) return 1;
  
  return n * factorial(n-1);
}
```



$ factorial(4)$ 

$ = 4*factorial(3) $ <br>$ = 4 * (3 * factorial(2)) $ <br>
$ = 4 * (3 * (2 * factorial(1))) $<br>
$= 4 * (3 * (2 * (1 * factorial(0)))$<br>
$ = 4 * (3 * (2 * (1 * 1)))$<br>
$ = 4 * (3 * (2 * 1)) $<br>
$ = 4 * (3 * 2) $<br>
$ = 4 * 6 $<br>
$ = 24 $



<br><br>



### 예시 2. GCD (Great common divisor)

$ gcd(x, y) = \cases { x & (y = 0) \cr gcd(y, remainder(x, y)) & (y > 0) } $



- Input: 자연수 $ x, y (  x \geq y,  y > 0 $ )
- Output: $x$ 와 $y$의 최대공약수
  - y가 0일 때 $x$
  - y가 0이 아닐때  $gcd (y, x\  mod\ y) $



<br>

#### 유클리드 호제법

78696과 19332의 최대공약수를 구하면,

```
78696 ＝ 19332×4 ＋ 1368
19332 ＝ 1368×14 ＋ 180
 1368 ＝ 180×7 ＋ 108
  180 ＝ 108×1 ＋ 72
  108 ＝ 72×1 ＋ 36
   72 ＝ 36×2 ＋ 0
```

따라서, 최대공약수는 36이다.

<br>

##### 정리

$ a, b \in Z $ 이고 $a$를 $b$로 나눈 나머지가 $r$이라고 하자. ($ a \geq b, \ 0 \leq r < b$) <br>$a$와 $b$의 최대공약수를 $gcd(a, b)$ 라고하면 다음이 성립한다. <br>$gcd(a, b) = gcd(b, r) $ <br>위의 78696과 19332에 대한 예시를 위와 같은 표현으로 적어보면, <br>$ gcd(78696, 19332) = gcd(19332, 1368) = ... = gcd(72, 36) = gcd(36, 0) = 36$



<br>

##### 증명 (Proof by Contradiction)

$ a, b \in Z $ 이고 $ a \geq b $ 이라고 하자. <br>그러면 $a = bq + r$ 을 만족하는 유일한 정수 $q, r$이 존재한다. 이 때  $0 \leq r < b$ 이다. <br>$gcd(a, b) = d, \  a = d\alpha, \  b = d\beta$ 라고 하자. 즉, $\alpha$ 와 $\beta$ 는 서로소이다.



$ a = bq + r   $ <br>=>  $d\alpha = d\beta q + r. $ <br>=> $d$ \| $r$  ($r$은 $d$의 배수) 

<br>

이제 $r = d\gamma$ 라고 하자. <br>만약 $gcd(\beta, \gamma) = d^{\prime} > 1$이라면,  $\beta = d^{\prime}\beta^{\prime}, \ \gamma = d^{\prime}\gamma^{\prime} $ 으로 두었을 때, <br>$ a = bq + r   $

$ => d\alpha = d\beta q +  d\gamma = dd^{\prime}\beta^{\prime}q + dd^{\prime}\gamma^{\prime} = dd^{\prime}(\beta^{\prime}q + \gamma^{\prime}) $ <br>$ => \alpha = d^{\prime}(\beta^{\prime}q + \gamma^{\prime}). $  <br>이 되므로, $ d^{\prime}$ \| $\alpha $ 이다. (즉, $\alpha$는 $d^{\prime}$의 배수)

<br>

즉, $ d^{\prime}$ \| $\alpha $,   $ d^{\prime}$ \| $\beta $ 가 되어 $\alpha$ 와 $\beta$ 는 서로소라는 것에 모순이다. <br>이는 $ gcd(\beta, \gamma) = d^{\prime} > 1 $ 이라는 가정에서 나타나는 모순이므로 $ gcd (\beta, \gamma) = 1 $ 이다. <br>따라서 $ gcd (b, r) =  d $ 이므로 $ gcd(a, b) = gcd(b, r) = d $

<br>



$gcd(a, b) = gcd(b, r) $ 구조에서 재귀과정이 보이고 <br>$gcd(a, b) = b $ 구조에서 기저단계가 보인다. <br>위 내용을 종합하여 코드를 작성하면 아래와 같습니다.

```c++
int gcd(x, y) {
  if (y == 0) return x;
  return gcd (y, x%y);
}
```



$gcd(259, 111) $ <br>
$ = gcd(111, 259\% 111) $ <br>
$ = gcd(111, 37) $  <br>$= gcd(37, 111\%37) $ <br>$= gcd(37,0) $
$= 37 $

<br>

<br>



### 예시 3. Recursive binary search

정렬된 배열에서 특정 값을 검색합니다.  <br>특정 값이 배열에 있다면 배열의 index를 반환하고 그렇지 않다면 FAILURE(-1)를 반환합니다.



#### Example

![array](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-11-recursion/array.png?raw=true)



- base case
  - target 값이 배열안에 존재하지 않을 때
  - target 값을 찾았을 때
- recursive case
  - target < 배열의 최댓값: 다시 배열을 반으로 나눕니다.
  - target > 배열의 최댓값: 반으로 나눴을 때, 남은 배열에서 다시 찾습니다.



<br>
$
bsearch(list, target, left, right) = \cases { left+right & (target = left+right) \cr bsearch(list, target, left, left+right) & (target < left+right) \cr bsearch(list, target, left+right+1, right) & (target > left+right) }
$

<br>


이분 탐색에 대한 자세한 내용은 이 포스트에서 다룹니다.

```c
int binarySearch(int list[], int target, int left, int right) {
    if (left > right) return FAILURE; // base case
    
    mid = (left + right)/2; 
    
    if (target == list[mid]) return mid; // base case
    
    if (target < list[mid])
        return binarySearch(list, target, left, mid-1); // recursive
    
    else
        return binarySearch(list, target, mid+1, right); // recursive
}
```





<br>

<br>

### 예시 4. 하노이 타워

세 개의 축과 $n$개의 원반이 주어지는데 각각의 원반은 크기가 다릅니다. 축을 A, B, C라고 부르기로 하고 원반은 가장 작은 원반을 1로, 가장 큰 원반을 $n$이라고 번호를 매긴다고 합시다. 처음에는 모든 $n$개의 원반이 크기 순서대로 위에서 아래로, 즉 1이 가장 위에, 그리고 $n$이 가장 아래인 상태로 축 A에 놓여 있습니다. 원반이 5개인 하노이 탑은 다음과 같습니다.

![hanoi](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-11-recursion/hanoi.png?raw=true)



**목표는 모든 $n$개의 원반을 축 A에서 축 B(혹은 C)로 옮기는 것입니다.**



![hanoi2](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-11-recursion/hanoi2.png?raw=true)







**규칙**

1. 한 번에 하나의 원반만 움직일 수 있습니다.
2. 자신보다 작은 원반이 그 아래에 놓일 수 없습니다. 예를 들어 원반 3이 축에 있다면 원반 3 밑에 있는 원반은 모두 3보다 큰 숫자로 되어 있어야 합니다.



<br>

#### 문제해결방법

i. 크기가 1부터 n-1인 디스크들을 A에서 C로 이동시킵니다.<br>(크기가 가장 큰 디스크를 옮기기 위해선 그보다 작은 모든 디스크를 모두 옮겨야합니다.)<br>



ii. 가장 큰 디스크를 A에서 C로 이동시킵니다.<br>iii. 옮겨놨던 나머지 디스크들 (크기가 1부터 n-1인 디스크들)을 B에서 C으로 이동시킵니다.<br>

![hanoi3](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-11-recursion/hanoi3.png?raw=true)

<br>

#### 재귀과정

$n$의 크기를 1씩 줄여가며 $n$이 1이 될때까지 과정을 반복한다면 하노이탑 문제를 해결할 수 있습니다.

- Base case: 크기가 1인 디스크를 옮길 때
- Recursive case: 크기가 2 이상인 디스크를 옮길 때



$ hanoi(n, from, to, via) = \cases { 1 & (n = 1) \cr hanoi(n-1, from, via, to) & (n > 1) }$



```c
void hanio (int n, char from, char via, char to) {
    if (n == 1) printf("Disk 1 from %c to %c\n", from , to);
    else {
        hanoi(n-1, from, to, via);
        printf("Disk %d from %c to %c\n", n, from, to);
        hanoi(n-1, via, from, to);
    }
}
```



<br>

#### **효율성**

N개의 원반이 있을 때 총 몇 번 이동시켜야 할까요? 

1. 상위 n-1 디스크를 A에서 B로 이동시킵니다.			   	`hanoi(n-1) move`
2. 나머지 (가장 큰) 디스크를 A에서 C로 이동시킵니다.        `hanoi(1) move`
3. n-1 디스크를 B에서 C으로 이동합니다.                           `hanoi(n-1) move`



$ hanoi(n) = \cases { 1 & (n = 1) \cr 2*hanoi(n-1)+1 & (n > 1) }$

 



<br>

<br>

### 예시 5. Fibonacci

$ fib(n) = \cases { 1 & (n $<$ 3) \cr fib(n-1) + fib(n-2) & (n $\geq$  3) }$



```c++
int fib (int n) {
  if (n < 3) return 1;
  
  return fib(n-1) + fib(n-2);
}
```





<br><br>



### 예시 6. Binomial coefficents

이항계수 페이지에서 다룹니다.







<br>

<br>

### **재귀는 좋은가?**

- 일반적으로 스택을 유지하는 오버헤드 때문에 더 느립니다.
- 스택을 사용하기 때문에 더 많은 메모리를 사용합니다.

=> 그럼에도 불구하고 문제해결을 더욱 간단하고 직관적으로 표시 할 수 있기 때문에 사용합니다.






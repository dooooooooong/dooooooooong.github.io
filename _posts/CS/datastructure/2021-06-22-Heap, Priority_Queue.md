---
layout: post
class: post-template
navigation: true
title: "14. Heap, Prioirty Queue"
date: 2021-06-22 16:20:20
categories: CS/DataStructure
use_math: true
---


## Heap

힙은 완전 이진 트리의 일종으로 **priority queue를 구현하기 위해** 만들어진 자료구조

<br>

**특징**

- 여러 개의 값들 중에서 최댓값이나 최솟값을 빠르게 찾아내도록 만들어진 자료구조이다.
- 반정렬 상태(느슨한 정렬 상태)
  - 큰 값이 상위 레벨에 있고 작은 값이 하위 레벨에 있다
  - 부모 노드의 키 값이 자식 노드의 키 값보다 항상 큰(작은) 이진 트리
- 힙 트리에서는 중복된 값을 허용한다. (이진 탐색 트리에서는 중복된 값을 허용하지 않는다.)



<br>

### Priority Queue

우선 순위가 있는 큐, 데이터들이 우선순위를 가지고 있고 우선순위가 높은 데이터가 먼저 나간다.



Ex) 은행에서 줄을 설 때 먼저 온 순서대로(FIFO) 서비스를 받을 수 있습니다. 하지만 노인이나 장애인이 줄을서는 경우 그들에게 우선권을 부여합니다. 사회적 약자는 다른 사람들보다 우선 순위가 높습니다.



**힙으로 구현한 우선순위 큐**

| 연산                     | 시간복잡도    |
| ------------------------ | ------------- |
| 최우선 element 가져오기  | $ O(1) $      |
| element 삽입             | $ O(\log N) $ |
| 최우선 element 요소 삭제 | $ O(\log N) $ |
| element 우선 순위 낮추기 | $ O(\log N) $ |

우선순위 큐는 배열, 연결리스트, 힙으로 구현할 수 있는데 그중 힙이 가장 효율적입니다.


<br>


**우선 순위 대기열 사용예시**

- Event-driven simulation.  [customers in a line, colliding particles]
- Numerical computation. [reducing roundoff error]
- Data compression. [Huffman codes]
- Graph searching. [Dijkstra's algorithm, Prim's algorithm]
- Number theory. [sum of powers]
- Artificial intelligence. [A* search]
- Statistics. [maintain largest M values in a sequence]
- Operating systems. [load balancing, interrupt handling]
- Discrete optimization. [bin packing, scheduling]
- Spam filtering. [Bayesian spam filter]




<br>


### Binary heap

힙으로 정렬 된 완전한 이진 트리의 배열 표현

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708201711626.png?raw=true" alt="image-20210708201711626" style="zoom:50%;" /> 



- 힙 순서: parent의 key는 child의 key보다 작지 않습니다. [max heap]
- 힙 구조: complete binary tree
- 배열 표현
  - index는 1부터 시작합니다.
  - 레벨 순서대로 노드를 가져옵니다.
    - k의 부모는 k / 2에 있습니다.
    - k의 아이들은 2k와 2k + 1입니다.


<br>
<br>

### Min-heap

부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진 트리

$ key(parent) \leq key(child) $

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708210024927.png?raw=true" alt="image-20210708210024927" style="zoom:50%;" />




<br>


#### **Insert**

- 힙 구조를 유지하면서 새 요소 삽입
- 힙 순서를 만족하지 않으면 서 요소를 힙 위로 이동



Example) key가 14인 노드 insert

<br>

i. 빈 자리를 찾아 우선 넣는다. (제일 마지막 자리)

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708210231377.png?raw=true" alt="image-20210708210231377" style="zoom:50%;" />


<br><br>


ii. 자신의 부모와 비교해서 자신이 더작으면 올라간다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708210441917.png?raw=true" alt="image-20210708210441917" style="zoom:50%;" />



<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708210622739.png?raw=true" alt="image-20210708210622739" style="zoom:50%;" />

<br><br>

iii. 자신의 부모보다 같거나 작으면 멈춘다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708210817319.png?raw=true" alt="image-20210708210817319" style="zoom:50%;" />



밑에서 올라가는 모양이 헤엄쳐 올라가는 모양과 비슷해서 **swim** 이라고 한다.



<br><br><br>

#### **Delete (dequeue)**

힙의 삭제는 루트의 삭제와 같다. (우선순위가 젤 높으므로)



<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708211119930.png?raw=true" alt="image-20210708211119930" style="zoom:50%;" />



<br><br>



i. 맨 마지막 노드를 지우려는 루트 자리에 덮어쓴다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708211521780.png?raw=true" alt="image-20210708211521780" style="zoom:50%;" />



<br><br>

ii. 자신의 자식 노드 중 작은 자식과 비교했을 때, 더 크면 내려온다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708211856821.png?raw=true" alt="image-20210708211856821" style="zoom:50%;" />





<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708211950808.png?raw=true" alt="image-20210708211950808" style="zoom:50%;" />





<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708212050195.png?raw=true" alt="image-20210708212050195" style="zoom:50%;" />



<br><br>

iii. leaf node가 되거나 자식들보다 작아지면 멈춘다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210708212210257.png?raw=true" alt="image-20210708212210257" style="zoom:50%;" /> 



위에서 내려가는 모양이 가라앉는 모양과 비슷해서 **sink** 이라고 한다.





<br><br>



### Max-heap

부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진 트리
<br>
$ key(parent) \geq key(child) $


<br><br>
삽입

i. 가장 마지막 자리에 추가한다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709001501433.png?raw=true" alt="image-20210709001501433" style="zoom:50%;" />


<br>
ii. 자신의 부모와 비교해서 자신이 더 크면 올라간다.



<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709001643148.png?raw=true" alt="image-20210709001643148" style="zoom:50%;" />

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709001755693.png?raw=true" alt="image-20210709001755693" style="zoom:50%;" />


<br>
iii. 자신의 부모보다 같거나 작으면 멈춘다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709001851827.png?raw=true" alt="image-20210709001851827" style="zoom:50%;" />


<br><br>
삭제

i. 루트 노드랑 마지막 노드랑 바꾼 후 삭제

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709004413255.png?raw=true" alt="image-20210709004413255" style="zoom:50%;" />

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709004612974.png?raw=true" alt="image-20210709004612974" style="zoom:50%;" />


<br><br>
ii. 자신의 자식 노드 중 큰 자식과 비교했을 때, 더 작으면 내려온다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709005002511.png?raw=true" alt="image-20210709005002511" style="zoom:50%;" />



<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709005112716.png?raw=true" alt="image-20210709005112716" style="zoom:50%;" />

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709010121013.png?raw=true" alt="image-20210709010121013" style="zoom:50%;" />


<br><br>
iii. 자신의 자식들보다 같거나 크면 멈춘다.

<img src="https://github.com/doooooooong/studyBoard/blob/master/Data-structures/DS%20note/images/image-20210709010233231.png?raw=true" alt="image-20210709010233231" style="zoom:50%;" />


<br><br><br>
#### 구현코드

```c
typedef int Key;
typedef struct heap_struct *heap;
typedef struct heap_struct {
  Key *node; // an array of nodes
  int capacity; // array size of node or key, item
  int N; // the number of nodes in the heap
} heap_struct;


void less (heap h, int i, int j) {
  
}

void more(heap, int i , int j) {
  
}

void swap (heap h, int p, int c) {
  Key item = h[p];
  h[p] = h[c];
  h[c] = item;
}

void swim (heap h, int k) {
  while (k > 1 && less(h, k/2, k)) {
    swap(h, k/2, k);
    k /= 2;
  }
}

void sink (heap h, int k) {
  swap (h, h->node[h->--N], 1);
  while () {
    
  } 
}
```




<br><br>
#### **시간복잡도**

N개의 노드가 있을 때 힙의 레벨은 $ \lfloor \log_2 N \rfloor $

- insert: $ O(\log N) $
- delete: $ O(\log N) $
- decreaseKey: $ O(\log N) $
- increaseKey: $ O(\log N) $
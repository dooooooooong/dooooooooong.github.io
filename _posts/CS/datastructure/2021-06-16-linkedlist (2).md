---
layout: post
class: post-template
navigation: true
title: "8. Linkedlist (2)"
date: 2021-06-16 14:15:23
categories: CS/DataStructure
use_math: true
---

실제 코드를 살펴봅시다.


<br>

#### 구조체 정의

```c++
// linkedlist.h
struct Node {
	int		data;
	Node*	next;
	// constructor with default arguments
	Node(int i = 0, Node* n = nullptr) { 
		data = i, next = n;
	}
	/* initializer list /////////////////////
	Node() : data(0), next(nullptr) {};
	Node(int i) : data(i), next(nullptr) {};
	Node(int i, Node* n): data(i), next(n) {};
	*////////////////////////////////////////
	~Node() {}
};

using pNode = Node*;
```





#### 기본 메소드

```c++
pNode clear(pNode p);	// free linked nodes and returns nullptr
pNode last(pNode p);	// returns the last node
bool empty(pNode p);	// true if empty, otherwise false
int size(pNode p);		// returns size in the list
void minmax(pNode p, int& min, int& max); // sets min, max of the list
```



##### clear (pNode p)

연결리스트를 삭제합니다.

```c++
pNode clear(pNode p) {
	if (empty(p)) return nullptr;

	pNode curr = p;
	while (curr != nullptr) {
		pNode prev = curr;
		curr = curr->next;
		delete prev;
	}
	return nullptr;
}
```



##### int size(pNode p)

연결리스트의 크기를 반환합니다.

```c++
int size(pNode p) {
	if (empty(p)) return 0;
	int count = 0;
	for (pNode c = p; c != nullptr; c = c->next, count++);
	return count;
}
```



##### void minmax(pNode p, int& min, int& max)

연결리스트의 노드의 키값중 최댓값과 최소값을 reference로 반환합니다.

```c++
void minmax(pNode p, int& min, int& max) {
	if (empty(p)) {
		min = max = 0;
		return;
	}

	min = INT_MAX, max = INT_MIN;

	Node * curr = p;
	while (curr != nullptr) {
		if (curr->data > max) max = curr->data;
		if (curr->data < min) min = curr->data;
		curr = curr->next;
	}

	return;
}
```



##### pNode last(pNode p)

연결리스트의 마지막 노드를 반환합니다.

```c++
pNode last(pNode p) {
	if (empty(p)) return nullptr;
	while (p->next != nullptr) p = p->next;
	return p;
}
```



##### empty(pNode p)

연결리스트가 비어있는지 확인합니다.

```c++
bool empty(pNode p) {
	return p == nullptr;
}
```





#### 추가 메소드

```c++
pNode push_front(pNode p, int x);	// pushes a new node at the front
pNode push_back(pNode p, int x);	// pushes a new node at the back
pNode push(pNode p, int val, int x);// pushes a new node at the position x
pNode push_N(pNode p, int N, pNode (*push_fp)(pNode, int)); // pushes N nodes
pNode push_sorted(pNode p, int val);// pushes a new node in sorted order ascending
pNode insertion_sort(pNode p);      // sorts and returns a new singly linked list
```



##### pNode push_front(pNode p, int x)

key값이 x인 노드를 연결리스트 맨 처음부분에 추가합니다.

```c++
pNode push_front(pNode p, int val) {
	if (empty(p)) return new Node{ val };
	return new Node{ val, p };
}
```



##### pNode push_back(pNode p, int x)

key값이 x인 노드를 연결리스트 마지막 부분에 추가합니다.

```c++
pNode push_back(pNode p, int val) {
    
	if (empty(p)) return new Node { val };

	pNode curr = p;
	while (curr->next != nullptr) {
		curr = curr->next;
	}

	curr->next = new Node { val, curr->next };

	return p;
}
```



##### pNode push(pNode p, int val, int x)

key값이 x인 기존 노드 앞에 key값이 val인 새로운 노드를 추가합니다.

만약 key값이 x인 기존 노드가 없다면 리스트의 마지막에 추가합니다.

```c++
pNode push(pNode p, int val, int x) {
	if (empty(p)) return push_front(p, val);
	if (p->data == x) return push_front(p, val);

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != x) {
		prev = curr;
		curr = curr->next;
	}

	if (curr->data != x) return push_back(p, val);

	else {
		pNode aNode = new Node { val };
		prev->next = aNode;
		aNode->next = curr;
	}

	return p;
}
```





##### pNode push_N(pNode p, int N, pNode (*push_fp)(pNode, int))

key값이 0 ~ (리스트의 크기+N)의 범위를 갖는 노드를 N개 생성하여 맨 앞 / 맨 뒤에 삽입합니다.

추가 위치는 함수포인터로 설정할 수 있습니다.

```c++
pNode push_N(pNode p, int N, pNode (*push_fp)(pNode, int)) {
	int range = N + size(p);
	srand((unsigned)time(nullptr));

	for (int i = 0; i < N; i++)
		p = push_fp(p, rand_extended() % range);

	return p;
}
```



##### pNode push_sorted(pNode p, int val)

정렬된 순서로 노드를 추가합니다.

노드가 5-6-2-4-7-10-3 이라면 새 노드 8을 추가한다면 새 노드의 위치는 7과 10의 사이가 됩니다.

5-6-2-4-7-<span style="color:red">8</span>-10-3

```c++
pNode push_sorted(pNode p, int val) {
	if (empty(p) || val <= p->data) return push_front(p, val);

	pNode curr = p;
	pNode prev = nullptr;
	// locate the node before the point of insertion

	int flag = 0;
	
	while (curr->next != nullptr) {
		prev = curr;
		curr = curr->next;

		if (prev->data < val && curr->data >= val) {
			flag = 1;
			break;
		}
	}

	if (curr->next == nullptr && flag == 0) {
		return push_back(p, val);
	}

	pNode aNode = new Node { val };
	aNode->next = prev->next;
	prev->next = aNode;

	return p;
}
```





##### pNode insertion_sort(pNode p)

리스트를 오름차순으로 정렬합니다.

```c++
pNode insertion_sort(pNode p) {
	if (empty(p)) return nullptr;
	if (size(p) < 2) return p;

	pNode curr = p;
	pNode sorted = nullptr;

	while(curr != nullptr) {
		sorted = push_sorted(sorted, curr->data);
		curr = curr->next;
	}

	return sorted;
}
```





#### 삭제 메소드

```c++
pNode pop_front(pNode p);			// pops the first node in the list
pNode pop_back(pNode p);			// pops the last node in the list
pNode pop(pNode p, int val);	    // pops the node with the val
pNode pop_N(pNode p, int N, pNode (*pop_fp)(pNode));  // pops N nodes
```



##### pNode pop_front(pNode p)

연결리스트의 첫 노드를 삭제합니다.

```c++
pNode pop_front(pNode p) {
	if (size(p) < 2) return nullptr;

	pNode aNode = p;
	p = p->next;
	delete aNode;

	return p;
}
```



##### pNode pop_back(pNode p)

연결리스트의 마지막 노드를 삭제합니다.

```c++
pNode pop_back(pNode p) {
	if (size(p) < 2) return nullptr;

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr) {
		prev = curr;
		curr = curr->next;
	}

	delete curr;
	prev->next = nullptr;

	return p;
}
```



##### pNode pop(pNode p, int val)

key값이 val인 노드를 찾아 삭제합니다.

```c++
pNode pop(pNode p, int val) {
	if (empty(p)) return nullptr;    // nothing to delete

	if (p->data == val) return pop_front(p);

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != val) {
		prev = curr;
		curr = curr->next;
	}

	if (curr->data == val && curr->next == nullptr) return pop_back(p);

	if (curr->data == val && curr->next != nullptr) {
		prev->next = curr->next;
		delete curr;
	}

	return p;
}
```





##### pNode pop_N(pNode p, int N, pNode (*pop_fp)(pNode))

N개의 노드를 연결리스트의 최전방 / 최후방으로부터 삭제합니다.

앞/뒤의 선택은 함수포인터를 이용합니다.

```c++
pNode pop_N(pNode p, int N, pNode (*pop_fp)(pNode)) {
	if (size(p) <= N) return nullptr;
	
	for (int i = 0; i < N; i++)
		p = pop_fp(p);

	return p;
}
```







#### 고급 기능

```c++
pair<pNode, pNode> cut_in_two_halves(pNode p); // cut the list in two halves
pNode reverse_using_stack(pNode p); // reverses list using stack
pNode reverse_in_place(pNode p);	// reverses list in-place
pNode zap_duplicates(pNode p);      // zap duplicates 
```



##### pair<pNode, pNode> cut_in_two_halves(pNode p)

연결리스트를 반으로 잘라서 두개의 연결리스트를 반환합니다.

연결리스트의 크기가 홀수라면 두번째 연결리스트의 크기가 1 크게 반환합니다. Ex) 1-2 / 3-4-5

```c++
pair<pNode, pNode> cut_in_two_halves(pNode p) {

	if (size(p) == 1) return make_pair(p, p);

	int n = size(p) / 2;
	int count = 0;

	pNode curr = p;
	pNode prev = nullptr;
	while (count != n) {
		prev = curr;
		curr = curr->next;
		count++;
	}

	prev->next = nullptr;
	pNode p2 = curr; 

	return make_pair(p, p2);  // p's are just place-holders.
}
```





##### pNode reverse_using_stack (pNode head)

 스택을 이용하여 연결리스트를 역순으로 뒤집습니다.

```c++
pNode reverse_using_stack(pNode head) {
	if (empty(head)) return nullptr;    // nothing to reverse
	
	stack < pNode > s;
	pNode curr = head;

	while (curr->next != nullptr) {
		s.push(curr);
		curr = curr->next;
	}

	head = curr;

	while (!s.empty()) {
		curr->next = s.top();
		curr = curr->next;
		s.pop();
	}

	curr->next = nullptr;

	return head;
}
```





##### pNode reverse_in_place(pNode head)

추가 메모리 없이 연결리스트를 역순으로 뒤집습니다.

```c++
pNode reverse_in_place(pNode head) {
	if (empty(head)) return nullptr;    // nothing to reverse

	pNode prev = nullptr;
	pNode last = nullptr;
	pNode curr = head;

	while (curr != nullptr) {
        last = prev;
        prev = curr;
        curr = curr->next;
        prev->next = last;
    }

	return prev;
}
```





##### pNode zap_duplicates(pNode p)

동일한 key를 가진 노드를 모두 제거합니다.

![zap_duplicates](../../../assets/images/markdown_images/CS/datastructure/2021-06-16-linkedlist (2)/zap_duplicates.jpg)

```c++
pNode zap_duplicates(pNode p) {
	if (empty(p)) return nullptr;
	if (size(p) == 1) return p;

	pNode curr = p->next;
	pNode prev = p;

	while (curr != nullptr) {
		if (prev->data == curr->data) {
			pNode aNode = curr;
			prev->next = curr->next;
			curr = curr->next;
			delete aNode;
		}

		else {
			prev = curr;
			curr = curr->next;
		}
	}

	return p;
}
```





#### 시간복잡도

| 작업                  | 시간복잡도 | 작업                 | 시간복잡도 |
| --------------------- | ---------- | -------------------- | ---------- |
| f - push front        | O(1)       | p - pop front        | O(1)       |
| b - push back         | O(n)       | y - pop back         | O(n)       |
| i - push              | O(n)       | d - pop              | O(n)       |
| B - push back N       | $O(n^2)$   | Y - pop back N       | $O(n^2)$   |
| F - push front N      | O(n)       | P - pop front N      | O(n)       |
| o - push sorted       | O(n)       | t - reverse in-stack | O(n)       |
| x - insertion sort    | $O(n^2)$   | r - reverse in-place | O(n)       |
| h - cut in two halves | O(n)       | z - zap duplicates   | O(n)       |







#### 전체코드

```bash
compile:	 g++ driver.cpp listnode.cpp -o linkedlist
Debug:		 g++ driver.cpp listnode.cpp -o linkedlist -DDEBUG
run: 		 ./linkedlist
```



##### driver.cpp

```c++
#include <iostream>
#include <string>
#include <ctime>

#include "nowic.h"
#include "listnode.h"
using namespace std;

void show_timeit(int begin) { 	// display elapsed time
	cout << "\tcpu: " << ((double)clock() - begin) / CLOCKS_PER_SEC << " sec\n";
}

int main() {
	char c;
	int val, min = 0, max = 0;
	clock_t begin = 0;
	Node *head = nullptr;
	bool show_all = false;        // toggle the way of showing values 
	string show_menu[] = { "HEAD/TAIL", "ALL" };
	int show_n = 12;              // n items shown per line  
	
	do {
		cout << "\n\tLinked List(nodes:" << size(head);
		cout << ", min,max:" << min << "," << max; 
		cout << ", show:" << show_menu[show_all] << "," << show_n << ")\n";

		cout << "\tf - push front     O(1)\t";   cout << "\tp - pop front        O(1)\n";
		cout << "\tb - push back      O(n)\t";   cout << "\ty - pop back         O(n)\n";
		cout << "\ti - push               \t";   cout << "\td - pop                  \n";
		cout << "\tB - push back  N       \t";   cout << "\tY - pop back  N          \n";
		cout << "\tF - push front N       \t";   cout << "\tP - pop front N          \n";
		cout << "\to - push sorted*       \t";   cout << "\tt - reverse using stack  \n";
		cout << "\tx - insertion sort*    \t";   cout << "\tr - reverse in-place     \n";
		cout << "\th - cut in two halves* \t";   cout << "\tz - zap duplicates*      \n";
		cout << "\tc - clear              \n";   
		if (show_all)
			cout << "\ts - show [HEAD/TAIL]\t";
		else
			cout << "\ts - show [ALL] items\t";
		cout << "\tn - n items per line   \n";

		c = GetChar("\tCommand[q to quit]: ");

		// execute the command
		switch (c) {
		case 'f':
		case 'b':
		case 'i':
			val = GetInt("\tEnter a number to push: ");
			switch (c) {
			case 'f':
				head = push_front(head, val);
				break;
			case 'b':
				head = push_back(head, val);
				break;
			case 'i':
				int x = GetInt("\tChoose a position node: ");
				head = push(head, val, x);
			}
			break;

		case 'p':  // deletes the first node in the list
			if (empty(head)) break;
			head = pop_front(head);
			break;

		case 'y':  // deletes the last node in the list.
			if (empty(head)) break;
			head = pop_back(head);
			break;

		case 'd':  // deletes the first node with val
			if (empty(head)) break;
			val = GetInt("\tEnter a number to delete: ");
			head = pop(head, val);
			break;

		case 't':  // reverses the list using stack.
			if (empty(head)) break;
			begin = clock();
			head = reverse_using_stack(head);
			show_timeit(begin);
			break;

		case 'r':  // reverses the list in-place.
			if (empty(head)) break;
			begin = clock();
			head = reverse_in_place(head);
			show_timeit(begin);
			break;

		case 'o':
			val = GetInt("\tEnter a number to push sorted: ");
			begin = clock();
			head = push_sorted(head, val);
			show_timeit(begin);
			break;

		case 'x':
			begin = clock();
			head = insertion_sort(head);
			show_timeit(begin);
			break;

		case 'Y': 
			if (empty(head)) break;
			val = GetInt("\tEnter number of nodes to pop back?: ");
			begin = clock();
			head = pop_N(head, val, pop_back);
			show_timeit(begin);
			break;

		case 'P': 
			if (empty(head)) break;
			val = GetInt("\tEnter number of nodes to pop front?: ");
			begin = clock();
			head = pop_N(head, val, pop_front);
			show_timeit(begin);
			break;

		case 'B': 
			val = GetInt("\tEnter number of nodes to push back?: ");
			begin = clock();
			head = push_N(head, val, push_back); 
			show_timeit(begin);
			break;

		case 'F': 
			val = GetInt("\tEnter number of nodes to push front?: ");
			begin = clock();

			head = push_N(head, val, push_front);   
			show_timeit(begin);
			break;

		case 's': // toggle the way of showing
			show_all ? show_all = false : show_all = true;
			break;

		case 'n':
			val = GetInt("\tEnter the max items to show per line: ");
			if (val >= 1) show_n = val;   
			break;

		case 'c':
			if (empty(head)) break;
			head = clear(head);
			break;

		case 'h':
			if (empty(head)) break;
			if (size(head) < 2) break;

			{
				auto two_heads = cut_in_two_halves(head);
				cout << "Showing 1st half & keeping 2nd half:\n";
				head = two_heads.second;	// sets the second half as head
				show(two_heads.first, show_all, show_n);
				cout << endl;

				clear(two_heads.first); // discard
			}
			break;

		case 'z':
			begin = clock();
			head = zap_duplicates(head);
			show_timeit(begin);
			break;
		
		default:
			break;
		}
		show(head, show_all, show_n);
		minmax(head, min, max);
	} while (c != 'q');

	clear(head);
	cout << "\n\t--Happy Coding!--\n";
	return EXIT_SUCCESS;
}

```



##### listnode.cpp

```c++
#include <iomanip>
#include <cstdlib>
#include <stack>
#include "listnode.h"
#include "rand.h"

#if 0
// a basic stack functinality only provided for pedagogical purpose
// To use STL stack, just comment out this section and inclucde <stack> above.
#include <vector>
template <typename T>
struct stack {
	vector<T> data;

	bool empty() const {
		return data.empty();
	}
	auto size() const {
		return data.size();
	}
	void push(T const& item) {
		data.push_back(item);
	}
	void pop() {
		if (data.empty())
			throw out_of_range("stack<>::pop(): pop stack");
		data.pop_back();
	}
	T top() const {
		if (data.empty())
			throw out_of_range("stack<>::top(): top stack");
		return data.back();
	}
};
#endif

// removes all the nodes from the list (which are destroyed),
// and leaving the container nullptr or its size to 0.
pNode clear(pNode p) {
	if (empty(p)) return nullptr;
	DPRINT(cout << "clear: ";);

	pNode curr = p;
	while (curr != nullptr) {
		pNode prev = curr;
		curr = curr->next;
		DPRINT(cout << prev->data << " ";);
		delete prev;
	}
	cout << "\n\tCleared...Happy Coding!\n";
	return nullptr;
}

// returns the number of nodes in the list.
int size(pNode p) {
	if (empty(p)) return 0;
	int count = 0;
	for (pNode c = p; c != nullptr; c = c->next, count++);
	return count;
}

// sets the min and max values that are passed as references in the list
void minmax(pNode p, int& min, int& max) {
	if (empty(p)) {
		min = max = 0;
		return;
	}

	min = INT_MAX, max = INT_MIN;

	Node * curr = p;
	while (curr != nullptr) {
		if (curr->data > max) max = curr->data;
		if (curr->data < min) min = curr->data;
		curr = curr->next;
	}

	return;
}

// returns the last node of in the list.
pNode last(pNode p) {
	if (empty(p)) return nullptr;
	while (p->next != nullptr) p = p->next;
	return p;
}

// returns true if the list is empty or no nodes.
// To remove all the nodes of a list, see clear().
bool empty(pNode p) {
	return p == nullptr;
}

// inserts a new node with val at the beginning of the list.
// This effectively increases the list size by one.
pNode push_front(pNode p, int val) {
	DPRINT(cout << "><push_front val=" << val << endl;);
	if (empty(p)) return new Node{ val };
	return new Node{ val, p };
}

// adds a new node with val at the end of the list and returns the
// first node of the list. This effectively increases the list size by one.
pNode push_back(pNode p, int val) {
	DPRINT(cout << "><push_back val=" << val << endl;);
	if (empty(p)) return new Node { val };

	pNode curr = p;
	while (curr->next != nullptr) {
		curr = curr->next;
	}

	curr->next = new Node { val, curr->next };

	return p;
}

// inserts a new node with val at the position of the node with x.
// The new node is actually inserted in front of the node with x.
// It returns the first node of the list.
// If the position x is not found, it inserts it at the end of the list.
pNode push(pNode p, int val, int x) {
	if (empty(p)) return push_front(p, val);
	if (p->data == x) return push_front(p, val);

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != x) {
		prev = curr;
		curr = curr->next;
	}


	if (curr->data != x) return push_back(p, val);

	else {
		pNode aNode = new Node { val };
		prev->next = aNode;
		aNode->next = curr;
	}

	return p;
}

// adds N number of new nodes at the front or back of the list.
// Values of new nodes are randomly generated in the range of
// [0..(N + size(p))]. 
// push_fp should be either a function pointer to push_front() 
// or push_back(). Use rand_extended() defined in nowic/include/rand.h.
pNode push_N(pNode p, int N, pNode (*push_fp)(pNode, int)) {
	DPRINT(cout << ">push_N = " << N << endl;);

	int range = N + size(p);
	srand((unsigned)time(nullptr));

	for (int i = 0; i < N; i++)
		p = push_fp(p, rand_extended() % range);

	DPRINT(cout << "<push_N = " << N << endl;);
	return p;
}

// inserts a new node in sorted ascending order in the list. 
// The basic strategy is to iterate down the list looking for the place to insert 
// the new node. That could be the end of the list, or a point just before a node 
// which is larger than the new node.
pNode push_sorted(pNode p, int val) {
	if (empty(p) || val <= p->data) return push_front(p, val);

	pNode curr = p;
	pNode prev = nullptr;
	// locate the node before the point of insertion

	// 들어갈 자리 찾으면 플래그 켜짐
	int flag = 0;
	
	while (curr->next != nullptr) {
		prev = curr;
		curr = curr->next;

		if (prev->data < val && curr->data >= val) {
			flag = 1;
			break;
		}
	}

	if (curr->next == nullptr && flag == 0) {
		return push_back(p, val);
	}

	pNode aNode = new Node { val };
	aNode->next = prev->next;
	prev->next = aNode;

	return p;
}

// sorts the singly linked list using insertion sortand returns a new list sorted.
// Repeatedly, invoke push_sorted() with a value in the list such that push_sorted() 
// returns a newly formed list head.
pNode insertion_sort(pNode p) {
	if (empty(p)) return nullptr;
	if (size(p) < 2) return p;

	pNode curr = p;
	pNode sorted = nullptr;

	while(curr != nullptr) {
		sorted = push_sorted(sorted, curr->data);
		curr = curr->next;
	}

	return sorted;
}

// removes the first node in the list and returns the new first node.
// This destroys the removed node, effectively reduces its size by one.
pNode pop_front(pNode p) {
	DPRINT(cout << ">pop_front size=" << size(p) << endl;);

	if (size(p) < 2) return nullptr;

	pNode aNode = p;
	p = p->next;
	delete aNode;

	return p;
}

// removes the last node in the list, effectively reducing the
// container size by one. This destroys the removed node.
pNode pop_back(pNode p) {
	DPRINT(cout << ">pop_back size=" << size(p) << endl;);
	if (size(p) < 2) return nullptr;

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr) {
		prev = curr;
		curr = curr->next;
	}

	delete curr;
	prev->next = nullptr;

	DPRINT(cout << "<pop_back size=" << size(p) << endl;);
	return p;
}

// deletes N number of nodes, starting from the beginning or back of the list. 
// It deletes all the nodes if N is zero which is the default or out of 
// the range of the list. 
pNode pop_N(pNode p, int N, pNode (*pop_fp)(pNode)) {
	DPRINT(cout << ">pop_N N=" << N << endl;);
	if (size(p) <= N) return nullptr;
	
	for (int i = 0; i < N; i++)
		p = pop_fp(p);

	DPRINT(cout << "<pop_N size=" << size(p) << endl);
	return p;
}

// removes the first occurrence of the node with val from the list
pNode pop(pNode p, int val) {
	DPRINT(cout << ">pop val=" << val << endl;);
	if (empty(p)) return nullptr;    // nothing to delete

	if (p->data == val) return pop_front(p);

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != val) {
		prev = curr;
		curr = curr->next;
	}

	if (curr->data == val && curr->next == nullptr) return pop_back(p);

	if (curr->data == val && curr->next != nullptr) {
		prev->next = curr->next;
		delete curr;
	}

	DPRINT(cout << "<pop size=" << size(p) << endl;);
	return p;
}

// reverses a singly linked list using stack and returns the new head. 
// The last node becomes the head node. This function pushes all of its 
// nodes onto a stack. Then top/pop stack and relink the nodes again. 
// It does not create any new node, but recycles and relink them.
// Even though it goes through the list twice, its time complexity is 
// still O(n).This algorithm, however, takes much longer time than 
// in-place reverse algorithm of which the time complexity is also O(n).
pNode reverse_using_stack(pNode head) {
	if (empty(head)) return nullptr;    // nothing to reverse
	
	stack < pNode > s;
	pNode curr = head;

	while (curr->next != nullptr) {
		s.push(curr);
		curr = curr->next;
	}

	head = curr;

	while (!s.empty()) {
		curr->next = s.top();

		curr = curr->next;
		s.pop();
	}

	curr->next = nullptr;

	return head;
}

// reverses a singly linked list and returns the new head. The last node
// becomes the head node. 
pNode reverse_in_place(pNode head) {
	if (empty(head)) return nullptr;    // nothing to reverse

	pNode prev = nullptr;
	pNode last = nullptr;
	pNode curr = head;

	while (curr != nullptr) {
        last = prev;
        prev = curr;
        curr = curr->next;
        prev->next = last;
    }

	return prev;
}

// The function cut_in_two_halves() cuts the list in two halves. 
// It returns the first node of the first half and the first node of the 
// second half of the list. If there are odd number of nodes, the second 
// half keeps one more node than the first half. For example, 
// the second half keeps 5 nodes if there are 9 nodes.
// the size of the list must be equal to or greater than two.
pair<pNode, pNode> cut_in_two_halves(pNode p) {

	if (size(p) == 1) return make_pair(p, p);

	int n = size(p) / 2;
	int count = 0;

	pNode curr = p;
	pNode prev = nullptr;
	while (count != n) {
		prev = curr;
		curr = curr->next;
		count++;
	}

	prev->next = nullptr;
	pNode p2 = curr; 

	return make_pair(p, p2);  // p's are just place-holders.
}

// removes consecutive items in the list, and leaves its neighbors unique.
// We can proceed down the listand compare adjacent nodes. When adjacent 
// nodes are the same, remove the second one.There's a tricky case where 
// the node after the next node needs to be noted before the deletion. 
// Your implementation must go through the list only once.
pNode zap_duplicates(pNode p) {
	if (empty(p)) return nullptr;
	if (size(p) == 1) return p;

	pNode curr = p->next;
	pNode prev = p;

	while (curr != nullptr) {
		if (prev->data == curr->data) {
			pNode aNode = curr;
			prev->next = curr->next;
			curr = curr->next;
			delete aNode;
		}

		else {
			prev = curr;
			curr = curr->next;
		}
	}

	return p;
}

// shows the values of all the nodes in the list if all is true or
// the list size is less than or equal to show_n * 2. If there are more than
// (show_n * 2) nodes, then it shows only show_n number of nodes from
// the beginning and the end in the list.
void show(pNode p, bool all, int show_n) {
	DPRINT(cout << "show(" << size(p) << ")\n";);
	if (empty(p)) {
		cout << "\n\tThe list is empty.\n";
		return;
	}

	int i;
	pNode curr;
	const int N = size(p);
	cout << endl << "TOP ";
	if (all || N <= show_n * 2) {
		for (i = 1, curr = p; curr != nullptr; curr = curr->next, i++) {
			cout << "\t" << curr->data;
			if (i % show_n == 0) cout << endl;
		}
		return;
	}
	
	// print the first show_n items
	curr = p;
	for (int i = 0; i < N-show_n; i++) {
		if (i >= show_n) {
			p = p->next;
			continue;
		}
		
		cout << "\t" << p->data;
		p = p->next;

	}

	cout << "\n...left out...\n";

	// print the last show_n items
	// Firstly, move the pointer to the place where show_n items are left.
	for (curr = p; curr != nullptr; curr = curr->next) {
		cout << "\t" << curr->data;
	}

	cout << endl;
}

```



##### listnode.h

```c++
//  
// listnode.h
// contains structures and functions for a simply-linked list of nodes.
// 
// 2018.12.12 Created by idebtor@gmail.com 
// 2020.04.15 Added reverse functions and push_N()

#ifndef LISTNODE_H
#define LISTNODE_H

#ifdef DEBUG
#define DPRINT(func) func;
#else
#define DPRINT(func) ;
#endif

#include <iostream>
#include <string>
using namespace std;

// Node struct contains data and a pointer to the next node.
struct Node {
	int		data;
	Node*	next;
	// constructor with default arguments
	Node(int i = 0, Node* n = nullptr) { 
		data = i, next = n;
	}
	/* initializer list /////////////////////
	Node() : data(0), next(nullptr) {};
	Node(int i) : data(i), next(nullptr) {};
	Node(int i, Node* n): data(i), next(n) {};
	*////////////////////////////////////////
	~Node() {}
};

using pNode = Node*;

Node* clear(Node* p);	// free linked nodes and returns nullptr
Node* last(Node* p);	// returns the last node
bool empty(Node* p);	// true if empty, otherwise false
int size(Node* p);		// returns size in the list
void minmax(Node* p, int& min, int& max); // sets min, max of the list

Node* push_front(Node* p, int x);	// pushes a new node at the front
Node* push_back(Node* p, int x);	// pushes a new node at the back
Node* push(Node* p, int val, int x);// pushes a new node at the position x
Node* push_N(Node* p, int N, Node* (*push_fp)(Node *, int)); // pushes N nodes
Node* push_sorted(Node* p, int val);// pushes a new node in sorted order ascending
Node* insertion_sort(Node* p);      // sorts and returns a new singly linked list

Node* pop_front(Node* p);			// pops the first node in the list
Node* pop_back(Node* p);			// pops the last node in the list
Node* pop(Node* p, int val);	    // pops the node with the val
Node* pop_N(Node* p, int N, Node* (*pop_fp)(Node*));  // pops N nodes

pair<Node*, Node*> cut_in_two_halves(Node* p); // cut the list in two halves
Node* keep_second_half(Node* p);	// keeps the second half
Node* reverse_using_stack(Node* p); // reverses list using stack
Node* reverse_in_place(Node* p);	// reverses list in-place
Node* zap_duplicates(Node* p);      // zap duplicates 

// if all is true, show all nodes; otherwise, show_n * 2 nodes from front & back. 
void show(Node* p, bool all = true, int show_n = 10);  // 10: a default magic number
#endif

```


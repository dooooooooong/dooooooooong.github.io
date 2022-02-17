---
layout: post
class: post-template
navigation: true
title: "7. Linkedlist"
date: 2021-06-15 20:19:13
categories: CS/DataStructure
use_math: true
---
각 노드에 자료 공간과 한 개의 포인터 공간이 있고, 각 노드의 포인터는 다음 노드를 가리킵니다.

<br>

![image-20220215163147655](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/linkedlist.png?raw=true)


구조체 타입으로 정의합니다.
```c++
struct Node {
	int		data; // item
	Node*	next; // pointer
};
using pNode = Node*;
```
사실 연결리스트의 내용은 이게 다입니다. 
노드와 노드가 연결된 자료구조에서 **노드추가, 탐색, 정렬, 삭제** 등의 여러가지 메소드와 세부사항에 대해 배워봅시다.

1부에서는 추가와 삭제에 대한 간단한 메커니즘과 코드를 배우고, 함수에 메소드 대한 자세한 내용은 2부에서 다룹니다.


<br><br>
### **단어 예제**

단어들이 임의의 순서로 주어졌을 때 단일 연결 리스트를 사용하여 알파벳 순으로 단어를 배열하고 싶습니다.  단어 목록은 다음과 같습니다.

HAT, CAT, EAT, WAT, BAT, FAT, VAT
![linked_list](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/linked_list.gif?raw=true)

알파벳순 정렬을 하며,  Link의 숫자(pointer)는 뒤에오는 data의 link입니다.
이 과정에서는 특별한 정렬 테크닉을 사용하지 않고 사람이 하나씩 정렬합니다.
```c
data[4] = bat;
link[4] = 1;

data[link[4]] = data[1] = cat;
link[1] = 2;

...
```



위와 같이 단일 연결 리스트란 각 노드가 데이터와 포인터를 가지고 한 줄로 연결되어 있는 방식으로 데이터를 저장하는 자료 구조입니다.
![image-20220215163444462](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/sorted_linkedlist.png?raw=true)

<br><br>



#### **추가**
새노드 `gat`를 연결 리스트에 삽입해봅시다.
**새 노드를 첫노드로 넣는 경우**
![addfirst](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/addfirst.jpg?raw=true)

```c++
pNode push_front(pNode p, int val) {
	if (empty(p)) return new Node { val };
	return new Node{ val, p };
}
```

<br>
**새 노드를 중간노드로 넣는 경우**
![addmiddle](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/addmiddle.jpg?raw=true)

```c++
// key 값이 x인 노드 뒤에 삽입하기
pNode push_Middle (pNode p, int val, int x) {
	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != x) {
		prev = curr;
		curr = curr->next;
	}

    pNode aNode = new Node { val };
    prev->next = aNode;
    aNode->next = curr;
	
	return p;
}
```

<br>
**새 노드를 마지막 노드로 넣는경우**
![addlast](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/addlast.jpg?raw=true)

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



<br><br>
종합하면 아래와 같습니다.
```c++
// key 값이 x인 노드 뒤에 삽입하기
pNode push(pNode p, int val, int x) {
	if (empty(p)) return push_front(p, val); // 맨 앞에 추가하기
	if (p->data == x) return push_front(p, val); // 맨 뒤에 추가하기

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != x) {
		prev = curr;
		curr = curr->next;
	}

	// 찾는 노드가 없다면 맨뒤에 추가하기
	if (curr->data != x) return push_back(p, val);

	else {
		pNode aNode = new Node { val };
		prev->next = aNode;
		aNode->next = curr;
	}

	return p;
}
```


<br><br>

**삭제**
노드 `gat`을 삭제해봅시다.
**첫 노드를 삭제하는 경우**

![rmfirst](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/rmfirst.jpg?raw=true)

```c++
pNode pop_front(pNode p) {
	if (size(p) <= 1) return nullptr;

	pNode aNode = p;
	p = p->next;
	delete aNode;

	return p;
}
```

<br>

**중간노드를 삭제하는 경우**

![rmmiddle](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/rmmiddle.jpg?raw=true)

```c++
// key값이 val인 노드를 찾아 삭제합니다.
pNode pop(pNode p, int val) {
	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != val) {
		prev = curr;
		curr = curr->next;
	}

    prev->next = curr->next;
    delete curr;

	return p;
}
```

<br>
**마지막 노드를 삭제하는 경우**

![rmlast](https://github.com/dooooooooong/dooooooooong.github.io/blob/master/assets/images/markdown_images/CS/datastructure/2021-06-15-linkedlist/rmlast.jpg?raw=true)

```c++
pNode pop_back(pNode p) {
	if (size(p) <= 1) return nullptr;

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




<br>
종합하면 아래와 같습니다.
```c++
// key값이 val은 노드를 찾아 삭제하기

pNode pop(pNode p, int val) {
	if (empty(p)) return nullptr;    // nothing to delete
	
    if (p->data == val) return pop_front(p); // 맨 앞의 노드 삭제

	pNode curr = p;
	pNode prev = nullptr;

	while (curr->next != nullptr && curr->data != val) {
		prev = curr;
		curr = curr->next;
	}

	if (curr->data == val && curr->next == nullptr) return pop_back(p); // 맨 뒤의 노드 삭제
	
    if (curr->data == val && curr->next != nullptr) {
		prev->next = curr->next;
		delete curr;
	}

	return p;
}
```


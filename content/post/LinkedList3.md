---
title: "[DataStructure]Linked List - 3"
date: 2021-11-25T22:07:32+09:00
draft: false
summary: "Circular Linked List와 Double Linked List"
---

# Circular Linked List

이전에 구현했던 Singly Linked List(단순 연결 리스트)와는 다르게 마지막 노드가 첫 노드와 연결되어 있는 리스트를 의미합니다.

마지막 노드가 처음 노드를 가르키고 있어 마치 원형과 같은 형태를 띄기 때문에 Circular Linked List(원형 연결 리스트)라고 부릅니다.

## 단순 연결 리스트와 원형 연결 리스트를 비교했을 때 원형 연결 리스트가 가지는 장점

단순 연결리스트로 만약 노드에 끝에 접근하고 싶다면 head부터 끝까지 탐색하거나 tail을 가르키는 포인터 변수를 하나 만들어서 가르키게 해야합니다.

하지만 원형 연결 리스트를 사용하면 head를 사용하지 않고 tail로 끝을 가르키게 하고 tail->next와 같은 접근으로 시작과 끝을 log(1)로 접근할 수 있습니다.

# Double Linked List

double linked list는 이전 노드의 위치와 다음 노드의 위치를 둘 다 가지고 있는 연결 리스트입니다.

양방향으로 탐색이 가능하기 때문에 노드를 탐색할 때 단순 연결리스트에 비해 훨씬 쉽고 빠르게 접근할 수 있지만 이전 위치를 가르키고 있는 포인터 만큼 메모리 공간을 사용해야 한다는 단점이 있습니다.

# Double Linked List in c

```c
typedef struct DoublyListNodeType
{
	int data;							// 데이터
	struct DoublyListNodeType* pLLink;	// 이전 노드의 주소
	struct DoublyListNodeType* pRLink; // 다음 노드의 주소
} DoublyListNode;

typedef struct DoublyListType
{
	int	currentElementCount;		// 현재 노드의 개수
	DoublyListNode	headerNode;		// 시작점(Header Node)
} DoublyList;
```


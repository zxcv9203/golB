---
title: "[DataStructure]Linked List - 5"
date: 2021-11-26T18:44:22+09:00
draft: false
summary : "Double Linked List의 역순"
---

# Double Linked List를 역순으로 바꾸기

Double Linked List를 역순으로 바꾸는 것은 매우 간단합니다.

Double Linked List의 노드들의 현재 가르키고 있는 이전 주소와 현재 주소를 스왑해주면 됩니다.

다음 예시를 살펴봅시다.

```
prev : 4 - node : 0 - next : 1
prev : 0 - node : 1 - next : 2
prev : 1 - node : 2 - next : 3
prev : 2 - node : 3 - next : 4
prev : 3 - node : 4 - next : 0
```

위와 같은 형태의 Double Linked List가 있을 경우 이전을 가리키고 있는 prev와 노드의 다음을 가리키고 있는 next를 전부 스왑해봅시다.
```
prev : 1 - node : 0 - next : 4
prev : 2 - node : 1 - next : 0
prev : 3 - node : 2 - next : 1
prev : 4 - node : 3 - next : 2
prev : 0 - node : 4 - next : 3
```

위의 결과를 보면 Double Linked List가 reverse 되는 것을 확인할 수 있습니다.

## reverse Double Linked List in c

```c
void reverseDoublyLinkedList(DoublyList *pList) {

	DoublyListNode *node;
	DoublyListNode *prev;
	DoublyListNode *tmp;

	if (pList == NULL)
		return ;
	prev = &pList->headerNode;
	node = pList->headerNode.pRLink;
	while (node != &pList->headerNode)
	{
		tmp = prev->pRLink;
		prev->pRLink = prev->pLLink;
		prev->pLLink = tmp;
		prev = node;
		node = node->pRLink;
	}
	tmp = prev->pRLink;
	prev->pRLink = prev->pLLink;
	prev->pLLink = tmp;
}
```

## 함수 흐름
1. 전달받은 매개변수가 NULL이면 함수를 종료합니다.
	``` c
	if (pList == NULL)
		return ;
2. 마지막 노드 이전까지 현재노드에서 이전노드를 가르키는 포인터와 다음노드를 가르키는 포인터를 스왑합니다.
	``` c
	prev = &pList->headerNode;
	node = pList->headerNode.pRLink;
	while (node != &pList->headerNode)
	{
		tmp = prev->pRLink;
		prev->pRLink = prev->pLLink;
		prev->pLLink = tmp;
		prev = node;
		node = node->pRLink;
	}
3. 마지막 노드를 스왑하고 종료합니다.
	``` c
	tmp = prev->pRLink;
	prev->pRLink = prev->pLLink;
	prev->pLLink = tmp;
---
title: "[DataStructure]Linked List - 4"
date: 2021-11-25T22:42:28+09:00
draft: false
summary : "Double Linked List의 구현"
---

# Double Linked List 구현 함수

DoublyList* createDoublyList()
* Double Linked list를 생성하는 함수

void deleteDoublyList(DoublyList* pList)
* Double Linked list를 삭제하는 함수(메모리 할당 해제)

int addDLElement(DoublyList* pList, int position, DoublyListNode element)

* Double Linked list를 position 위치에 elment의 내용으로 추가합니다.

int removeDLElement(DoublyList* pList, int position);
* Double Linked list의 포지션 위치의 값을 삭제합니다.

void clearDoublyList(DoublyList* pList);
* Double linked list의 data를 초기화합니다.

int getDoublyListLength(DoublyList* pList);
* Double Linked list의 현재 길이를 가져옵니다.

DoublyListNode* getDLElement(DoublyList* pList, int position);
* Double Linked List의 position 위치의 노드를 반환합니다.

void displayDoublyList(DoublyList* pList);
* Display Double List의 값들을 출력해줍니다.

## DoublyList* createDoublyList()

Double Linked list를 생성하는 함수입니다.

``` c
DoublyList* createDoublyList()
{
	DoublyList *dl;

	dl = (DoublyList *)malloc(sizeof(DoublyList));
	if (dl == NULL)
		return (NULL);
	dl->currentElementCount = 0;
	dl->headerNode.data = 0;
	dl->headerNode.pLLink = NULL;
	dl->headerNode.pRLink = NULL;
	return (dl);
}
```

### 함수 흐름

1. double linked list를 동적할당합니다.
	``` c
	dl = (DoublyList *)malloc(sizeof(DoublyList));
	if (dl == NULL)
		return (NULL);
2. double linked list의 현재 개수와 노드의 값들을 초기화 한 후 반환합니다.
	``` c
	dl->currentElementCount = 0;
	dl->headerNode.data = 0;
	dl->headerNode.pLLink = NULL;
	dl->headerNode.pRLink = NULL;
	return (dl);

## void deleteDoublyList(DoublyList* pList)

Double Linked list를 삭제하는 함수(메모리 할당 해제)

``` c
	DoublyListNode *node;
	DoublyListNode *release;

	if (pList == NULL)
		return ;
	node = pList->headerNode.pRLink;
	while (node && node != &pList->headerNode) {
		release = node;
		node = node->pRLink;
		release->pLLink = 0;
		release->pRLink = 0;
		release->data = 0;
		free(release);
	}
	pList->currentElementCount = 0;
	pList->headerNode.pLLink = 0;
	pList->headerNode.pRLink = 0;
	free(pList);
```
### 함수 흐름

1. 매개변수로 전달받은 Double Linked List가 NULL일 경우 함수를 종료합니다.
	```c
	if (pList == NULL)
		return ;
2. 0번째 노드부터 메모리 할당을 해제 하기 위해 node를 0번째 노드를 가르키도록 합니다.
	``` c
	node = pList->headerNode.pRLink;
3. node에 연결되어 있는 함수들을 해제합니다. (node != &pList->headerNode의 이유는 맨마지막 노드는 header node를 가르키고 있으므로 node가 headerNode를 가르키게 된다면 모든 노드를 다 탐색한 상황이 됩니다.) 
	``` c
	while (node && node != &pList->headerNode) {
	release = node;
	node = node->pRLink;
	release->pLLink = 0;
	release->pRLink = 0;
	release->data = 0;
	free(release);
	}
4. 마지막으로 pList의 동적할당을 해제합니다.
	``` c
	pList->currentElementCount = 0;
	pList->headerNode.pLLink = 0;
	pList->headerNode.pRLink = 0;
	free(pList);

## int addDLElement(DoublyList* pList, int position, DoublyListNode element)

Double Linked list의 노드를 position 위치에 element의 내용으로 추가하는 함수입니다.

``` c
int addDLElement(DoublyList* pList, int position, DoublyListNode element)
{
	DoublyListNode *node;
	DoublyListNode *prev;
	int				i;

	i = 0;
	if (pList == NULL)
		return (FALSE);
	if (position < 0 || pList->currentElementCount < position)
		return (FALSE);
	node = (DoublyListNode *)malloc(sizeof(DoublyListNode));
	if (node == NULL)
		return (FALSE);
	node->data = element.data;
	prev = &pList->headerNode;
	while (i++ < position && prev->pRLink)
		prev = prev->pRLink;
	node->pRLink = prev->pRLink;
	node->pLLink = prev;
	prev->pRLink = node;
	if (node->pRLink == NULL || node->pRLink == &pList->headerNode)
	{
		node->pRLink = &pList->headerNode;
		pList->headerNode.pLLink = node;
	}
	pList->currentElementCount++;
	return (TRUE);
}
```

### 함수 흐름

1. 매개변수로 받은 Double Linked List가 NULL 이라면 실패했다는 것을 알리기 위해 FALSE(0)을 반환하고 종료합니다.
	``` c
	if (pList == NULL)
		return (FALSE);
2. 매개변수로 받은 node의 위치를 가리키는 position 변수가 음수거나 현재 노드의 개수보다 크다면 잘 못된 위치 값이므로 FALSE를 반환하고 종료합니다.
	```c
	if (position < 0 || pList->currentElementCount < position)
		return (FALSE);
3. 추가할 노드를 동적할당하고 실패할 경우 FALSE를 반환하도록 합니다.
	```c
	node = (DoublyListNode *)malloc(sizeof(DoublyListNode));
	if (node == NULL)
		return (FALSE);
4. 추가할 노드에 매개변수로 받은 element의 데이터 값을 넣습니다.
	``` c
	node->data = element.data;
5. 이전 노드까지 탐색합니다.
	```c
	prev = &pList->headerNode;
	while (i++ < position && prev->pRLink)
		prev = prev->pRLink;
6. node의 다음 주소가 node의 이전주소가 다음으로 가르키는 곳을 가르키게 합니다.
	```c
	node->pRLink = prev->pRLink;
7. node의 이전 주소로 prev(node 이전 위치)를 가르키게 합니다.
	``` c
	node->pLLink = prev;
8. node의 이전 주소를 가리키는 prev의 다음 주소를 node를 가르키게 합니다.
	``` c
	prev->pRLink = node;
9. 만약 노드의 다음주소로 가르키는 주소가 NULL이거나 headerNode라면 마지막 노드이기 때문에 node의 다음주소를 headerNode로 하고 헤더노드의 이전(마지막 노드) 주소를 마지막 노드의 주소로 합니다. 
	``` c
	if (node->pRLink == NULL || node->pRLink == &pList->headerNode)
	{
		node->pRLink = &pList->headerNode;
		pList->headerNode.pLLink = node;
	}
10. 노드의 개수를 1 증가시키고 종료합니다.
	``` c
	pList->currentElementCount++;
	return (TRUE);
## int removeDLElement(DoublyList* pList, int position)

### 함수 흐름

## void clearDoublyList(DoublyList* pList)

### 함수흐름

## int getDoublyListLength(DoublyList* pList)

### 함수 흐름

## DoublyListNode* getDLElement(DoublyList* pList, int position)

### 함수 흐름

## void displayDoublyList(DoublyList* pList)

### 함수 흐름
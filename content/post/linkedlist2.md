---
title: "[DataStructure]Linked List - 2"
date: 2021-11-24T15:28:18+09:00
draft: false
summary : "Singly Linked List의 구현"
---

# Linkded List 구현 함수

LinkedList* createLinkedList()

* 링크드리스트를 생성하여 그 포인터를 반환하는 함수

int addLLElement(LinkedList* pList, int position, ListNode element)

* pList의 원하는 위치(position)에 노드(element)를 삽입하는 함수

int removeLLElement(LinkedList* pList, int position);

* pList의 원하는 위치(position)에 있는 노드(element)를 제거하는 함수

ListNode* getLLElement(LinkedList* pList, int position);

* pList의 원하는 위치(position)에 있는 노드(element)를 가져오는 함수

void clearLinkedList(LinkedList* pList);

* pList의 data값을 0으로 초기화하는 함수

int getLinkedListLength(LinkedList* pList);

* pList의 길이를 구해서 반환하는 함수

void deleteLinkedList(LinkedList* pList);

* pList를 free()해서 링크드리스트를 삭제하는 함수

## LinkedList* createLinkedList()

Linked List를 생성하여 반환하는 함수입니다.

```c
LinkedList* createLinkedList()
{
    LinkedList *lst;

    lst = (LinkedList *)malloc(sizeof(LinkedList));
	if (lst == NULL)
		return (NULL);
	lst->currentElementCount = 0;
	lst->headerNode.pLink = NULL;
	return lst;
}
```

### 함수 흐름

1. 반환할 LinkedList를 선언하고 동적할당을 하며, 동적할당에 실패했을 경우 함수를 종료하고 NULL을 반환하도록 합니다.
	``` c
		LinkedList *lst;

		lst = (LinkedList *)malloc(sizeof(LinkedList));
		if (lst == NULL)
			return (NULL);
2. 현재 Element의 개수를 0으로 초기화합니다.
	``` c
		lst->currentElementCount = 0;
3. 다음 노드를 가르키는 주소를 NULL로 초기화합니다.
	``` c
		lst->headerNode.pLink = NULL;

## int addLLElement(LinkedList* pList, int position, ListNode element)

Linked List의 원하는 위치(position)에 노드(element)를 삽입하는 함수입니다.

``` c
int addLLElement(LinkedList* pList, int position, ListNode element)
{
    ListNode    *prev; // position 위치의 노드의 주소를 저장할 변수
    ListNode    *node; // 현재노드의 주소를 저장할 변수
    int         i;		// 인덱스 변수

    i = 0;
    if (pList == NULL)
        return (FALSE);
    if (pList->currentElementCount + 1 < position || position < 0)
        return (FALSE);
    node = (ListNode *)malloc(sizeof(ListNode));
    if (node == NULL)
        return (FALSE);
    node->data = element.data;
    prev = &pList->headerNode;
    while(i < position && prev->pLink)
    {
        prev = prev->pLink;
        i++;
    }
    node->pLink = prev->pLink;
    prev->pLink = node;
    pList->currentElementCount++;
    return (TRUE);
}
```

### 함수 흐름

1. 만약 매개변수로 받은 Linked List가 NULL이라면 종료합니다.

	```c
	if (pList == NULL)
		return (FALSE);
2. pList의 현재 엘리먼트의 개수 + 1보다 position이 크거나 음수라면 제대로 된 위치가 아니라고 판단하고 함수를 종료합니다.

	``` c
	if (pList->currentElementCount + 1 < position || position < 0)
		return (FALSE);
3. 추가할 노드를 동적할당하고 할당에 실패했다면 함수를 종료합니다.

	``` c
	node = (ListNode *)malloc(sizeof(ListNode));
    if (node == NULL)
        return (FALSE);
4. node에 매개변수로 받은 element의 data 값을 넣습니다. (pLink를 넣지 않는 이유는 pLink를 넣을 경우 추가적인 처리가 필요함)

	``` c
    node->data = element.data;
5. prev는 pList의 headerNode의 주소를 받습니다.
	``` c
	prev = &pList->headerNode;
6. 해당 포지션에 도달할때까지 반복문을 이용하여 prev를 이동시킵니다. (prev->pLink를 추가한 이유는 만약 맨끝에 넣으려고 할때는 prev를 이동시키지않고 맨마지막 인덱스를 기준으로 사용하기 위함)

	``` c
	while(i < position && prev->pLink)
    {
        prev = prev->pLink;
        i++;
    }
7. 추가한 node의 다음 주소는 현재 position위치의 노드의 다음주소로 변경해줍니다.
	``` c
	node->pLink = prev->pLink;
8. position위치의 노드는 새로만든 node를 가르키게 변경해줍니다.
	``` c
	prev->pLink = node;
9. 현재 Element의 개수를 1 증가시킨 후 종료합니다.
	``` c
	pList->currentElementCount++;
	return (TRUE);

## int removeLLElement(LinkedList* pList, int position)

pList의 원하는 위치(position)에 있는 노드(element)를 제거하는 함수입니다.

``` c
int removeLLElement(LinkedList* pList, int position)
{
   ListNode    *node; // 삭제할 노드
   ListNode    *prev; // 이전 노드
   int          i;

   if (position >= pList->currentElementCount || position < 0)
       return (FALSE);
   if (pList == NULL)
       return (FALSE);
   prev = &pList->headerNode;
   i = 0;
   while (i++ < position && prev->pLink)  
		prev = prev->pLink;
   node = prev->pLink;
   prev->pLink = node->pLink;
   free(node);
   node = 0;
   pList->currentElementCount--;
   return (TRUE);
}
```

### 함수 흐름
1. position이 현재 Element의 개수보다 크거나 같거나 음수일 경우 잘못된 위치이므로 함수를 종료합니다.
	``` c
	if (position >= pList->currentElementCount || position < 0)
       return (FALSE);
2. 입력받은 Linked List가 NULL일 경우 함수를 종료합니다.
	``` c
	if (pList == NULL)
       return (FALSE);
3. 시작위치를 prev에 저장합니다.
	``` c
	prev = &pList->headerNode;
4. 삭제할 위치 이전까지 노드를 이동시킵니다.
	```c
	while (i++ < position && prev->pLink)  
		prev = prev->pLink;
5. 삭제할 위치를 node에 저장합니다.
	```c
	node = prev->pLink;
6. prev가 다음으로 가르키고 있는 위치를 node가 다음 위치로 가르키는 곳으로 변경 합니다.
	``` c
	prev->pLink = node->pLink;
7. 삭제를 원하는 위치를 할당 해제합니다.
	``` c
	free(node);
   	node = 0;
8. 현재 linked list의 개수를 1 감소 시킨 후 종료합니다.
	``` c
	pList->currentElementCount--;
	return (TRUE);
## ListNode* getLLElement(LinkedList* pList, int position)
Linked List의 원하는 위치(position)에 있는 노드를 가져오는 함수입니다.
```c
ListNode* getLLElement(LinkedList* pList, int position)
{
   ListNode    *node;

   if (pList == NULL)
       return (FALSE);
   if (position >= pList->currentElementCount || position < 0)
       return (FALSE);
   node = pList->headerNode.pLink;
   while (position--)
       node = node->pLink;
   return (node);
}
```
### 함수흐름

1. 매개변수로 받은 Linked List가 NULL일 경우 함수를 종료합니다.
	``` c
	if (pList == NULL)
       return (FALSE);
2. 가져올 노드의 위치인 position이 음수거나 현재 Element의 개수와 같거나 크다면 잘못 된 값이므로 함수를 종료합니다.
	```c
	if (position >= pList->currentElementCount || position < 0)
       return (FALSE);
3. 값을 가져올 노드의 시작점을 지정합니다.
	``` c
   node = pList->headerNode.pLink;
4. 원하는 위치까지 값을 이동한 후 해당 값을 반환합니다.
	```c
	while (position--)
       node = node->pLink;
	return (node);
## void clearLinkedList(LinkedList* pList)
Linked List에 연결된 node들의 data값을 0으로 초기화하는 함수입니다.
```c
void clearLinkedList(LinkedList* pList)
{
    ListNode    *node; // 초기화할 노드
    int     i;	// 인덱스 변수

    if (pList == NULL)
        return ;
    i = pList->currentElementCount;
    node = pList->headerNode.pLink;
    while (i--)
    {
        node->data = 0;
        node = node->pLink;
    }
}
```
### 함수 흐름
1. 매개변수로 주어진 Linked List가 NULL이라면 함수를 종료합니다.
	```c
	if (pList == NULL)
        return ;
2. 현재 노드의 개수와 노드의 시작 위치를 지정합니다.
	``` c
	i = pList->currentElementCount;
    node = pList->headerNode.pLink;
3. Linked List의 모든 노드를 0으로 초기화 시킵니다.
	``` c
	while (i--)
    {
        node->data = 0;
        node = node->pLink;
    }
## int getLinkedListLength(LinkedList* pList)
Linked List의 현재 노드의 개수를 구해서 반환하는 함수입니다.
``` c
int getLinkedListLength(LinkedList* pList)
{
    if (pList == NULL)
        return (FALSE));
    return (pList->currentElementCount);
}
```
### 함수 흐름
1. 매개변수로 받은 Linked List가 NULL이라면 함수를 종료합니다.
	``` c
	if (pList == NULL)
        return (FALSE));
2. Linked List의 현재 Element의 개수를 반환하고 종료합니다.
	``` c
    return (pList->currentElementCount);
## void deleteLinkedList(LinkedList* pList)
Linked List의 할당된 메모리를 해제하는 함수입니다.

``` c
void deleteLinkedList(LinkedList* pList)
{
	ListNode  *node;
	ListNode  *prev;

	if (pList == NULL)
		return ;
	node = pList->headerNode.pLink;
	while (node) 
	{
		prev = node;
		node = node->pLink;
		free(prev);
		prev = 0;
	}
	free(pList);
	pList = 0;
}
```

### 함수흐름

1. 매개변수로 전달 받은 Linked List가 NULL일 경우 함수를 종료합니다.
	``` c
	if (pList == NULL)
		return ;
2. node를 0번째의 Element를 가르키도록 만듭니다.
	``` c
	node = pList->headerNode.pLink;
3. node를 전부 탐색하며 메모리 할당을 해제합니다.
	``` c
	while (node) 
	{
		prev = node;
		node = node->pLink;
		free(prev);
		prev = 0;
	}
4. 동적할당한 Linked List를 해제합니다.

``` c
free(pList);
pList = 0;
```
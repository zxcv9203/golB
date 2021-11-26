---
title: "[DataStructure]Linked List - 6"
date: 2021-11-26T18:44:39+09:00
draft: false
summary: "Linked List를 이용한 다항식 계산"
---

# 다항식 계산

다항식을 계산할 때 Linked List를 이용하면 간편하게 계산할 수 있습니다.

다항식은 같은 차수의 값끼리 계산을 해야하고 다른 차수는 그대로 유지해야 합니다.

예를 들어 다음과 같은 두개의 다항식을 덧셈을 하면

```
차수 : 3 계수 : 2 , 차수 : 1 계수 : 7
차수 : 3 계수 : 5 , 차수 : 2 계수 : 1
```

다음과 같은 결과 값을 얻어야 합니다.

```
차수 : 3 계수 : 7 , 차수 : 2 계수 : 1 , 차수 : 1 계수 : 7
```

# 다항식 계산 구현 아이디어
먼저, 다항식 계산을 할 다항식을 링크드 리스트로 만듭니다.

그 다음 두개의 링크드 리스트를 매개변수로 받아 이 둘을 덧셈한 링크드리스트를 반환하도록 합니다.

매개변수로 받은 링크드 리스트는 각각의 iterator를 가지고 링크드 리스트의 노드를 끝까지 탐색하며, 각각의 iterator들이 가리키고 있는 값을 연산을 합니다.

링크드리스트는 node의 degree 값이 같은 경우에만 덧셈 연산을 하도록 하게 합니다.

degree 값이 다를 경우 큰 값을 먼저 반환할 리스트의 노드로 추가합니다.

값을 사용한 노드들은 계속해서 다음 노드로 이동하며, 한쪽이 먼저 끝났을 경우 남아있는 값들을 그대로 뒤에 이어붙입니다.

# 다항식 계산 링크드 리스트의 구조

```h
typedef struct ListNodeType
{
	float coef; // 계수
	int degree; //차수
	struct ListNodeType* pLink;	// next 포인터
} ListNode;

typedef struct LinkedListType
{
	int currentElementCount;	// 현재 Element의 개수
	ListNode headerNode;		// Element
} LinkedList;
```

# 다항식 계산 구현 in c

```c
static int	handle_remaining_term(LinkedList *poly, ListNode *node, int index)
{
	int		stat;

	while (node)
	{
		stat = addLLElement(poly, index, *node);
		if (stat == FALSE)
		{
			deleteLinkedList(poly);
			return (FALSE);
		}
		index++;
		node = node->pLink;
	}
	return (TRUE);
} 

LinkedList	*addPoly(LinkedList *lsA, LinkedList *lsB)
{
	LinkedList	*poly; // 다항식 계산 결과가 담기는 링크드 리스트
	ListNode	*IterA; // lsA의 노드를 가리키는 포인터
	ListNode	*IterB; // lsB의 노드를 가리키는 포인터
	ListNode	calc; // degree가 같을 경우 계산 결과가 들어가는 node
	int			stat; // 반환 값 있는 함수를 호출했을 때 해당 함수의 결과 값을 받아옵니다.
	int			i; // 인덱스 변수
	
	i = 0;
	if (lsA == NULL && lsB == NULL)
		return (NULL);
	else if (lsA == NULL)
		return (copyLinkedList(lsB));
	else if (lsB == NULL)
		return (copyLinkedList(lsA));
	poly = createLinkedList();
	if (poly == NULL)
		return (NULL);
	IterA = lsA->headerNode.pLink;
	IterB = lsB->headerNode.pLink;
	while(IterA && IterB)
	{
		if (IterA->degree > IterB->degree)
		{
			stat = addLLElement(poly, i, *IterA);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (NULL);
			}
			IterA = IterA->pLink;
		}
		else if (IterA->degree < IterB->degree)
		{
			stat = addLLElement(poly, i, *IterB);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (NULL);
			}
			IterB = IterB->pLink;
		}
		else // IterA->degree == IterB->degree 
		{
			calc.coef = IterA->coef + IterB->coef;
			calc.degree = IterA->degree;
			stat = addLLElement(poly, i, calc);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (NULL);
			}
			IterA = IterA->pLink;
			IterB = IterB->pLink;
		}
		i++;
	}
	stat = handle_remaining_term(poly, IterA, i);
	if (stat == FALSE)
		return (NULL);
	stat = handle_remaining_term(poly, IterB, i);
	if (stat == FALSE)
		return (NULL);
	return (poly);
}
```

## 함수 흐름

1. 매개변수로 받은 Linked List가 NULL인지 아닌지를 판단합니다. 한쪽만 NULL이라면 NULL이 아닌 Linked List를 깊은 복사를 해서 반환합니다.
	```c
	if (lsA == NULL && lsB == NULL)
		return (NULL);
	else if (lsA == NULL)
		return (copyLinkedList(lsB));
	else if (lsB == NULL)
		return (copyLinkedList(lsA));
2. 다항식 계산한 결과를 저장하는 링크드 리스트를 동적할당합니다.
	``` c
	poly = createLinkedList();
	if (poly == NULL)
		return (NULL);
3. 매개변수로 전달받은 리스트의 iterator를 만듭니다.
	```c
	IterA = lsA->headerNode.pLink;
	IterB = lsB->headerNode.pLink;
4. degree의 크기를 비교해서 같은 경우 계수의 값을 더해서 노드를 추가하고 다르다면 더 큰쪽의 계수의 값을 poly에 추가합니다. 값을 사용한 노드는 다음 노드로 이동시킵니다.
	``` c
	while(IterA && IterB)
	{
		if (IterA->degree > IterB->degree)
		{
			stat = addLLElement(poly, i, *IterA);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (NULL);
			}
			IterA = IterA->pLink;
		}
		else if (IterA->degree < IterB->degree)
		{
			stat = addLLElement(poly, i, *IterB);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (NULL);
			}
			IterB = IterB->pLink;
		}
		else // IterA->degree == IterB->degree 
		{
			calc.coef = IterA->coef + IterB->coef;
			calc.degree = IterA->degree;
			stat = addLLElement(poly, i, calc);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (NULL);
			}
			IterA = IterA->pLink;
			IterB = IterB->pLink;
		}
		i++;
	}
5. 만약 매개변수로 받은 리스트가 끝에 도달하지 않았으면 해당 내용을 뒤에 그대로 이어 붙입니다.
	``` c
	stat = handle_remaining_term(poly, IterA, i);
	if (stat == FALSE)
		return (NULL);
	stat = handle_remaining_term(poly, IterB, i);
	if (stat == FALSE)
		return (NULL);
	```
	```c
	static int	handle_remaining_term(LinkedList *poly, ListNode *node, int index)
	{
		int		stat;

		while (node)
		{
			stat = addLLElement(poly, index, *node);
			if (stat == FALSE)
			{
				deleteLinkedList(poly);
				return (FALSE);
			}
			index++;
			node = node->pLink;
		}
		return (TRUE);
	} 
6. 계산한 링크드 리스트를 반환합니다.
	``` c
	return (poly);
---
title: "[DataStructure]Array List - 3"
date: 2021-11-22T22:17:03+09:00
draft: false
summary: "Array List 구현하기 in C"
---

# Array List 함수

Array List 함수들은 다음과 같이 존재합니다.

* ArrayList *CreateArrayList(int maxElementCount)

* void     deleteArrayList(ArrayList* pList)

* int isArrayListFull(ArrayList* pList)

* int addALElement(ArrayList *pList, int position)

* int removeALElement(ArrayList *pList, int position)

* ArrayListNode* getALElement(ArrayList* pList, int position)

* void displayArrayList(ArrayList *pList)

* void clearArrayList(ArrayList* pList)

* int getArrayListLength(ArrayList *pList)

## ArrayList *CreateArrayList(int maxElementCount)

해당 함수는 maxElementCount 크기만큼의 배열을 가진 Array List를 만든 후 해당 Array List를 반환하는 함수입니다.

> 구현

``` c
ArrayList* createArrayList(int maxElementCount)
{
  ArrayList *arr;

	if (maxElementCount <= 0)
		return (NULL);
  arr = (ArrayList *)malloc(sizeof(ArrayList));
  if (arr == NULL)
      return (NULL);
  arr->maxElementCount = maxElementCount;
  arr->currentElementCount = 0;
  arr->pElement = (ArrayListNode *)malloc(sizeof(ArrayListNode) * maxElementCount);
  if (arr->pElement == NULL)
	{
		free(arr);
		arr = 0;
		return (NULL);
	}
	return (arr);
}
```

함수의 흐름은 다음과 같습니다.

1. 반환할 ArrayList를 생성합니다.
    
    ```c
    ArrayList *arr;
    ```
    
2. 만약 maxElementCount가 음수 혹은 0이 들어올 경우 정상적인 값이 아니기 때문에 NULL을 리턴합니다.
    
    ```c
    if (maxElementCount <= 0)
    		return (NULL);
    ```
    
3. 반환할 ArrayList를 동적할당 합니다. 만약 할당에 실패한 경우 NULL을 리턴합니다.
    
    ```c
    arr = (ArrayList *)malloc(sizeof(ArrayList));
      if (arr == NULL)
          return (NULL);
    ```
    
4. 반환할 ArrayList에 현재 element의 개수(넣은 값이 없기때문에 0)와 최대 element의 개수를 할당합니다.
    
    ```c
    arr->maxElementCount = maxElementCount;
    arr->currentElementCount = 0;
    ```
    
5. 반환할 ArrayList에 element를 저장할 배열을 동적할당 합니다. 배열의 크기는 maxElementCount 만큼 할당하며, 할당에 실패할 경우 반환할 ArrayList도 free 시킨후 NULL을 반환합니다.
    
    ```c
    arr->pElement = (ArrayListNode *)malloc(sizeof(ArrayListNode) * maxElementCount);
      if (arr->pElement == NULL)
    	{
    		free(arr);
    		arr = 0;
    		return (NULL);
    	}
    ```
    
6. 할당에 성공했다면 ArrayList를 반환합니다.
    
    ```c
    return (arr);
    ```

> 헤멘부분

malloc을 이용해서 동적할당을 할 경우 인덱스 범위가 동적할당한 크기를 넘어서더라도 접근이 가능합니다. (배열을 스택영역에 할당할 경우 인덱스를 넘어간 접근을 할 시 segmentaion fault가 발생)

하지만 접근을 할 수 있더라도 인덱스를 벗어난 값은 안전하지 않기 때문에 사용하지 않아야 합니다.

그래서 인덱스를 벗어난 접근을 하지 않도록 주의합시다.

## void     deleteArrayList(ArrayList* pList)

매개변수로 전달 된 Array List에 할당된 메모리를 전부 해제하는 함수입니다.

> 구현

`deleteArrayList` 함수는 다음과 같이 구현하였습니다.

```c
void		deleteArrayList(ArrayList* pList)
{
	if (pList->pElement == NULL)
		printf("pElement is null parameter\n");
	else
		free(pList->pElement);
	pList->pElement = 0;
	if (pList == NULL)
		printf("pElement is null parameter\n");
	else
		free(pList);
	pList = 0;
}
```

함수의 흐름은 다음과 같습니다.

1. 먼저, `pList→pElement`가 NULL일 경우에 대한 예외처리를 해줍니다.

	```c
	if (pList->pElement == NULL)
			printf("pElement is null parameter\n");
	```

2. `pList→pElement`가 NULL이 아니라면, free()를 통해 해당 메모리를 해제해줍니다다. 그리고 그 포인터 주소에 0을 넣어줍니다.

(이때, `pList→pElement`가 NULL인 경우에 `pList→pElement = 0`을 하게되어도, 이미 같은 값이므로 상관이 없습니다)

	```c
	else
			free(pList->pElement);
	pList->pElement = 0;
	```

3. `pList`가 NULL일 경우에 대한 예외처리를 해줍니다. 그리고 그게 아닌 경우에 대해서는, 메모리를 해제해주고 포인터 주소에 0을 넣어줍니다.

	```c
	if (pList == NULL)
			printf("pElement is null parameter\n");
		else
			free(pList);
		pList = 0;
	```

## int isArrayListFull(ArrayList* pList)

매개변수로 전달된 Array List의 Array가 가득 찼는지 확인하는 함수입니다.

함수가 가득 찼다면 1을 반환하고 함수가 가득차지 않았다면 0을 반환하고 잘못된 인자를 전달한 경우 -1을 반환합니다.

> 구현
> 

`isArrayListFull` 함수는 다음과 같이 구현하였습니다.

```c
int		isArrayListFull(ArrayList* pList)
{
	if (pList == NULL)
	{
		printf("pList is null parameter\n");
		return (ERROR);
	}
	if (pList->maxElementCount == pList->currentElementCount)
		return (TRUE);
	return (FALSE);
}
```

함수의 흐름은 다음과 같습니다.

1. 매개변수로 받은 `ArrayList`가 `NULL`이라면 `NULL`이 반환되었다는 것을 알리고 `ERROR`를 리턴합니다.
    
    ```c
    if (pList == NULL)
    	{
    		printf("pList is null parameter\n");
    		return (ERROR);
    	}
    ```
    
2. 만약 최대로 가질수 있는 `Element`의 개수가 현재 `Element`의 개수와 같다면 `Array`가 가득찬 것 이기 때문에 `TRUE`을 반환합니다.
    
    ```c
    if (pList->maxElementCount == pList->currentElementCount)
    		return (TRUE);
    ```
    
3. 2번을 실행했을 때 값이 다르다면 아직 공간이 남은 것이기 때문에 `FALSE`를 리턴합니다.
    
    ```c
    return (FALSE);
    ```

## int addALElement(ArrayList *pList, int position)

매개변수로 전달받은 Array List의 Array에 position의 위치에 Element를 삽입하는 함수입니다.

해당 position에 값이 존재한다면 해당 position의 위치를 밀어냅니다.

추가 성공시 TRUE, 실패시 FALSE를 반환합니다.

> 구현
함수의 흐름은 다음과 같습니다.

1. 매개변수로 받은 `ArrayList`가 `NULL`이라면 `NULL`이 반환되었다는 것을 알리고 `ERROR`를 리턴합니다.
    
    ```c
    if (pList == NULL)
    {
    	printf("pList is null parameter\n");
    	return (ERROR);
    }
    ```
    
2. `Array`의 인덱스를 가리키는 `position` 의 값이 음수거나 `Array`의 최대 크기를 넘어간다면 ERROR를 리턴합니다.
    
    ```c
    if (position < 0 || position >= pList->maxElementCount)
    {
    	printf("Invalid position value.\n");
    	return (ERROR);
    }
    ```
    
3. `Element`를 삽입하고자 하는 위치에 `data`가 존재하지 않을 경우, 해당 위치에 `Element`의 데이터를 넣어주고 현재 `Element`의 개수를 하나 증가시킵니다.
    
    ```c
    if (!pList->pElement[position].data)
    {
    	pList->pElement[position].data = element.data;
    	pList->currentElementCount++;
    }
    ```
    
4. 현재 `Element`의 개수가 최대 `Element`의 개수보다 더 작은 경우, `memmove()`를 통해 주어진 위치와 그 뒷부분의 전체적인 메모리를 한칸 씩 뒤로 밀어줍니다. 그리고 주어진 위치에 주어진 `Element`의 데이터를 할당합니다. 그리고 현재 `Element`의 개수를 하나 증가시킵니다.
    
    ```c
    else if (pList->maxElementCount > pList->currentElementCount)
    {
    	memmove(&pList->pElement[position + 1], &pList->pElement[position], sizeof(*pList) * (pList->maxElementCount - position - 1));
    	pList->pElement[position].data = element.data;
    	pList->currentElementCount++;
    }
    ```
    
5. 현재 `Element`의 개수가 최대 `Element`와 같은 경우, `realloc()`을 통해 메모리를 한칸 더 할당해준 뒤, 기존과 같은 `Element` 삽입 과정을 진행합니다.
    
    ```c
    else if (pList->currentElementCount == pList->maxElementCount)
    {
    	ArrayList *tmp = (ArrayList *)realloc(pList, sizeof(ArrayList));
    	if (!tmp)
    		return (0);
    	deleteArrayList(pList);
    	memmove(&tmp->pElement[position + 1], &tmp->pElement[position], sizeof(*pList) * (tmp->maxElementCount - position - 1));
    	tmp->pElement[position].data = element.data;
    	tmp->currentElementCount++;
    	pList = tmp;
    }
    ```

## int removeALElement(ArrayList *pList, int position)

매개변수로 전달받은 Array List의 Array에 position의 위치에 Element를 삭제하는 함수입니다.

해당 position 뒤에 값이 존재한다면 해당 position 뒤의 위치를 앞으로 땡겨옵니다.

삭제 성공시 TRUE, 실패시 FALSE를 반환합니다.

> 구현
> 

`removeALElement` 함수는 다음과 같이 구현하였습니다.

```c
int 	removeALElement(ArrayList* pList, int position)
{
	if (pList == NULL)
	{
		printf("pList is null parameter\n");
		return (ERROR);
	}
	if (position < 0 || position >= pList->maxElementCount)
	{
		printf("Invalid position value.\n");
		return (ERROR);
	}
	if (pList->currentElementCount == 0)
		return (FALSE);
	memmove(&pList->pElement[position], &pList->pElement[position + 1], sizeof(*pList) * (pList->maxElementCount - position - 1));
	pList->currentElementCount--;
	return (TRUE);	
}
```

함수의 흐름은 다음과 같습니다.

1. 매개변수로 받은 `ArrayList`가 `NULL`이라면 `NULL`이 반환되었다는 것을 알리고 `ERROR`를 리턴합니다.
    
    ```c
    	if (pList == NULL)
    	{
    		printf("pList is null parameter\n");
    		return (ERROR);
    	}
    ```
    
2. `Array`의 인덱스를 가리키는 `position` 의 값이 음수거나 `Array`의 최대 크기를 넘어간다면 ERROR를 리턴합니다.
    
    ```c
    if (position < 0 || position >= pList->maxElementCount)
    {
    	printf("Invalid position value.\n");
    	return (ERROR);
    }
    ```
    
3. 현재 `Element`의 개수가 0일 경우 삭제를 진행하는 것이 의미가 없으므로 `FALSE`를 반환합니다.
    
    ```c
    	if (pList->currentElementCount == 0)
    		return (FALSE);
    ```
    
4. ArrayList에서 삭제가 이루어지는 경우 해당 인덱스의 값을 삭제하고 그 이후에 존재하는 `Element`을 앞으로 가져와야 하기 때문에 이후의 값을 땡겨오도록 구현했습니다. 
    
    ```c
    memmove(&pList->pElement[position], 
    					&pList->pElement[position + 1], 
    						sizeof(*pList) * (pList->maxElementCount - position - 1));
    ```
    
    위의 과정을 그림으로 표현하면 다음과 같습니다.
    
    1. 다음과 같은 크기가 5인 배열이 있습니다.
        
        ```
		1 2 3 4 5
		```
        
    2. 이 상황에서 포지션이 2라면 3의 값이 이후의 값들로 다음과 같이 덮어쓰여집니다.
        
        ```
		1 2 4 5 X
		```
        
5. 현재 `Element`의 개수를 하나 감소시킨후 `TRUE`를 반환하고 종료합니다.
    
    ```c
    pList->currentElementCount--;
    return (TRUE);
    ```

## ArrayListNode* getALElement(ArrayList* pList, int position)

ArrayList의 특정 위치에 있는 Element의 주소를 반환하는 함수입니다.

> 구현

`getALElement` 함수는 다음과 같이 구현하였습니다.

```c
ArrayListNode* getALElement(ArrayList* pList, int position)
{
	if (pList == NULL)
		return (NULL);
	if (position >= 0 && position < pList->maxElementCount)
		return (NULL);
	return (&pList->pElement[position]);
}
```

1. pList가 NULL이 전달되었는지 확인하고 NULL이라면 NULL을 리턴합니다.

	``` c
	if (pList == NULL)
		return (NULL);
	```

2. position의 값이 정상적인 범위인지 확인하고 배열의 범위를 벗어났다면 NULL을 반환합니다.

	```c
	if (position >= 0 && position < pList->maxElementCount)
		return (NULL);
	```

3. Array의 position 위치의 값의 주소를 반환해줍니다.

	``` c
	return (&pList->pElement[position]);
	```
## void displayArrayList(ArrayList *pList)

Array List에 들어있는 Array의 Element를 출력해주는 함수입니다.

> 구현

`displyArrayList` 함수는 다음과 같이 구현하였습니다.

```c
void displayArrayList(ArrayList* pList)
{
	int i;

	i = 0;
	if (pList == NULL)
		return (NULL);
	while (i < pList->currentElementCount)
	{
		printf("[%d] : %d\n", i, pList->pElement[i].data);
		i++;
	}
}
```

함수의 흐름은 다음과 같습니다.

1. pList가 NULL인지 확인하고 NULL이라면 NULL을 반환합니다.
	``` c
	if (pList == NULL)
		return (NULL);
	```

2. Array의 인덱스를 가르킬 `i` 변수를 선언합니다.
    
    ```c
    int i;
    
    i = 0;
    ```
    
3. 현재 `Element`의 개수 만큼 `[position] : Element` 와 같은 형태로 한줄씩 출력합니다.
    
    ```c
    while (i < pList->currentElementCount)
    	{
    		printf("[%d] : %d\n", i, pList->pElement[i].data);
    		i++;
    	}
    ```

## void clearArrayList(ArrayList* pList)

매개변수로 받은 Array List에 있는 Array의 Element의 값을 0으로 초기화하는 함수입니다.

> 구현

`clearArrayList` 함수는 다음과 같이 구현하였습니다.

```c
void clearArrayList(ArrayList* pList)
{
	int i;

	i = 0;
	if (pList == NULL)
		return (NULL);
	while (i < pList->maxElementCount)
	{
		pList->pElement[i].data = 0;
		i++;
	}
}
```

1. pList가 NULL인지 확인하고 NULL이라면 NULL을 반환합니다.
	``` c
	if (pList == NULL)
		return (NULL);
	```
2. 반복문을 돌며 주어진 ArrayList에서의 Element의 데이터 값을 모두 초기화한다.
	``` c
	while (i < pList->maxElementCount)
	{
		pList->pElement[i].data = 0;
		i++;
	}
	```

## int getArrayListLength(ArrayList *pList)

Array List에 현재 들어있는 값을 출력해주는 함수입니다.

> 구현

`getArrayListLength` 함수는 다음과 같이 구현하였습니다.

```c
int getArrayListLength(ArrayList* pList)
{
	if (pList == NULL)
		return (NULL);
	return (pList->currentElementCount);
}
```

함수의 흐름은 다음과 같습니다.

1. pList가 NULL인지 확인하고 NULL이라면 NULL을 반환합니다.
	``` c
	if (pList == NULL)
		return (NULL);
	```

2. 현재 Array에 들어있는 Element의 개수를 반환합니다.
    
    ```c
    return pList->currentElementCount;
    ```
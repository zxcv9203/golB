---
title: "[DataStructure]Array List - 2"
date: 2021-11-22T22:12:34+09:00
draft: false
summary: "Array List에 대해서"
---

# Array List

Array List는 배열의 이용하여 리스트를 구현한 것을 의미합니다.

내부적으로 배열을 사용하기 때문에 인덱스를 이용해서 접근하는 것이 빠르지만 데이터의 추가와 삭제가 느립니다.

## Array List에서의 데이터 삽입

Array List는 내부적으로 데이터를 배열에 저장합니다.

배열에 현재 위치에 값이 존재할 경우 값들을 뒤로 밀어내고 해당 자리에 들어가야 합니다.

```c
1 2 3 4 // 1번째 위치에 5를 추가
1 5 2 3 4 // 2 3 4가 뒤로 밀려남
```

위 같은 경우 다음과 같은 순서로 이루어집니다.

1. 삽입할 자료만큼 공간을 늘리는 작업을 합니다.
2. 삽입할 자료의 위치를 기준으로 기존의 데이터들을 뒤로 이동하는 연산을 수행합니다.
3. 삽입할 위치에 자료가 입력되면 삽입 연산을 마칩니다.

## Array List에서의 데이터 삭제

배열에 빈 공간이 생기면 빈자리를 채우기 위해서 순차적으로 한칸씩 땡겨야합니다.

```c
1 2 3 4 // 1번째 위치의 값을 삭제
1 3 4 // 3 4를 앞으로 땡겨옴
```

1. 삭제할 자료가 위치한 인덱스의 자료를 삭제합니다.
2. 삭제한 자료의 인덱스를 기준으로 이후의 자료들을 이동하는 연산을 합니다.
3. List의 맨 마지막은 비어있는 상태를 완료합니다.

## C in Array List

저는 C에서 Array List를 다음과 같은 형태로 구현하였습니다.

```c
typedef struct ArrayListNodeType
{
	int data;               // 배열의 원소
} ArrayListNode;

typedef struct ArrayListType
{
	int maxElementCount;		// 원소의 최대 개수
	int currentElementCount;	// 현재 원소의 개수
	ArrayListNode *pElement;	// 원소를 저장할 1차원 배열
} ArrayList;
```

* maxElementCount : 원소가 들어갈 수 있는 최대 개수(배열의 크기)
* currentElementCount : 현재 들어있는 원소의 개수
* *pElement : 원소가 저장되는 1차원 배열
* data : 배열의 원소
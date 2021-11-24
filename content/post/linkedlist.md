---
title: "[DataStructure]Linked List - 1"
date: 2021-11-23T00:15:05+09:00
draft: false
summary: "Linked List란?"
---

# Linked List란?

Linked List는 Array List와는 다르게 Element와 Element 간의 연결(link)를 이용해서 리스트를 구현한 것을 의미합니다.

## Linked List와 Array List의 차이
Array List는 메모리 공간에 연속적으로 값이 저장되고

Linked list는 다음 주소를 가리키는 변수가 존재합니다.

다음과 같은 배열이 있다면

```
1 2 3 4 5
```

배열의 경우 다음과 같이 연속된 공간으로 메모리에 할당될 것입니다.

```
X X X X X X X X X X
X X X X X X X X X X
X X X X X X X X X X
1 2 3 4 5 X X X X X
X X X X X X X X X X
X X X X X X X X X X
X X X X X X X X X X
X X X X X X X X X X
X X X X X X X X X X
X X X X X X X X X X
```

링크드 리스트의 경우 다음과 같이 저장되게 됩니다.

```
X X X X X X X X X X
X X X 1 X X X X 3 X
X X X X X X X X X X
X X X X X X X X X X
X X X X X 2 X X X X
X X X X X X X X X X
X X X X X X X X X X
X 4 X X X X X X X X
X X X X X X X 5 X X
X X X X X X X X X X
```

그렇기 때문에 인덱스 접근이 불가능하고 다음 값이 어디 존재하는지 가르키고 있어야 합니다.

하지만 데이터를 추가하고 삭제할 때 인접한 데이터가 가르키는 주소만 변경하면 되기 때문에 해제하고 재할당한 후 각각의 Element를 복사해야 하는 Array List보다 추가 / 삭제가 빠릅니다.
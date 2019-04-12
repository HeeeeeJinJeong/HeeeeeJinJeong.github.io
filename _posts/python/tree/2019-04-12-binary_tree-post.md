---
title: "binary tree"
date: 2019-04-12
categories: tree
---

# binary tree
사이클이 없는 연결된 그래프(connected acyclic graph)

- degree(차수) : 어떤 노드의 자식 노드의 개수
- degree of a tree(트리의 차수) : 트리에 있는 노드의 최대 차수
- leaf node : 차수가 0인 노드, 자식이 없는 노드(단말 노드 라고도 부름)
- level : root의 레벨을 1로 하고 자식으로 내려가면서 하나씩 더한다.
- 트리의 높이(heigth) or 깊이(depth) : 트리가 가지는 최대 레벨
- forest : 노트 노드를 없앤 후 얻은 서브 트리의 집합
- edge : 노드 사이를 연결하는 선, link라고도 부름
- 노드와 엣지의 관계 : e = n - 1 (노드의 개수 : n, 엣지의 개수 : e)

## 이진 트리의 특징
- 레벨 l에서 최대 노드수 : 2의 l-1승 개
- 높이가 h인 이진 트리의 최대 노드 수 : 2의 h-1승 개
- 높이가 h인 이진 트리의 최소 노드 수 : h개

## 이진 트리의 종류
1. 포화 이진 트리 (full binary tree) : 모든 레벨이 꽉 차 있는 트리
2. 완전 이진 트리 (complete binary tree) : 트리의 노드가 위에서 아래로, 왼쪽에서 오른쪽으로 채워지는 트리
3. 편향 이진 트리 (skewed binary tree) : 왼쪽이나 오른쪽 서브 트리만 가지는 트리

## 트리의 순회
- stack 계열 : DFS(Depth First Search)
1. 전위 순회(preorder traversal) : 노드 -> 왼쪽 서브 트리 -> 오른쪽 서브 트리
2. 중위 순회(inorder traversal) : 왼쪽 서브 트리 -> 노드 -> 오른쪽 서브 트리
3. 후위 순회(postorder traversal) : 왼쪽 서브 트리 -> 오른쪽 서브 트리 -> 노드


- queue 계열 : BPS(Brouth First Search)
1. 레벨 순회(level order traversal) : 레벨 순서대로 순회


```python
from stack import Stack
from queue import Queue

class TreeNode:
    def __init__(self,data):
        self.__data=data
        self.__left=None
        self.__right=None

    @property
    def data(self):
        return self.__data

    @data.setter
    def data(self,data):
        self.__data=data

    @property
    def left(self):
        return self.__left

    @left.setter
    def left(self,left):
        self.__left=left

    @property
    def right(self):
        return self.__right

    @right.setter
    def right(self,right):
        self.__right=right

# recursion
def preorder(cur):
    # 기저 조건
    if not cur:
        return

    # 방문 : 데이터 출력
    # cur 방문
    print(cur.data, end='  ')
    preorder(cur.left)
    preorder(cur.right)

def inorder(cur):
    if not cur:
        return

    inorder(cur.left)
    print(cur.data, end='  ')
    inorder(cur.right)

def postorder(cur):
    if not cur:
        return

    postorder(cur.left)
    postorder(cur.right)
    print(cur.data, end='  ')


def iter_preorder(cur):
    stack=LStack()

    while True:
        while cur:
            print(cur.data, end='  ') # 스택에 넣기 전에 cur 방문
            stack.push(cur)
            cur=cur.left
        
        cur=stack.pop()

        # stack이 비었을 때
        if not cur:
            break

        cur=cur.right

def iter_inorder(cur):
    stack=LStack()

    while True:
        while cur:
            stack.push(cur)
            cur=cur.left
            
        cur=stack.pop()
        
        # stack이 비었을 때
        if not cur:
            break

        print(cur.data, end='  ')
        cur=cur.right

def iter_postorder(cur):
    s1=LStack()
    s2=LStack()

    s1.push(cur)
    while not s1.empty():
        cur=s1.pop()
        s2.push(cur)

        if cur.left:
            s1.push(cur.left)

        if cur.right:
            s1.push(cur.right)

    while not s2.empty():
        cur=s2.pop()
        print(cur.data, end='  ')

def levelorder(cur):
    queue=LQueue()

    queue.enqueue(cur)
    while not queue.empty():
        cur=queue.dequeue()
        print(cur.data, end='  ')

        if cur.left:
            queue.enqueue(cur.left)

        if cur.right:
            queue.enqueue(cur.right)


if __name__=="__main__":
    n1=TreeNode(1)
    n2=TreeNode(2)
    n3=TreeNode(3)
    n4=TreeNode(4)
    n5=TreeNode(5)
    n6=TreeNode(6)
    n7=TreeNode(7)

    n1.left=n2; n1.right=n3
    n2.left=n4; n2.right=n5
    n3.left=n6; n3.right=n7

    # preorder(n1) 
    # print() # 1  2  4  5  3  6  7

    # inorder(n1)
    # print() # 4  2  5  1  6  3  7

    # postorder(n1)
    # print() # 4  5  2  6  7  3  1


    # iter_preorder(n1)
    # print() # 1  2  4  5  3  6  7

    # iter_inorder(n1)
    # print() # 4  2  5  1  6  3  7

    # iter_postorder(n1)
    # print() # 4  5  2  6  7  3  1

    levelorder(n1)
    print()
```

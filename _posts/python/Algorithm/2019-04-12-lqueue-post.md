---
title: "lqueue"
date: 2019-04-12
categories: algorithm
---

# lqueue

```python
class Node:
    def __init__(self, data=None):
        self.__data=data
        self.__next=None

    @property
    def data(self):
        return self.__data
    
    @data.setter
    def data(self, data):
        self.__data=data
    
    @property
    def next(self):
        return self.__next

    @next.setter
    def next(self, n):
        self.__next=n

class LQueue:
    def __init__(self):
        self.front=None
        self.rear=None

    def empty(self):
        if self.front==None:
            return True
        else:
            return False

    def enqueue(self,data):
        new_node=Node(data)

        if self.empty():
            self.front=new_node
            self.rear=new_node

        self.rear.next=new_node
        self.rear=new_node

    def dequeue(self):
        if self.empty():
            return

        # 데이터가 하나 있을 땐 rear를 옮겨준다
        if self.front is self.rear:
            self.rear=self.rear.next

        cur=self.front
        self.front=self.front.next
        return cur.data


    def peek(self):
        if self.empty():
            return

        return self.front.data

if __name__ == "__main__":
    q = LQueue()
    q.enqueue(1)
    q.enqueue(2)
    q.enqueue(3)
    q.enqueue(4)
    q.enqueue(5)

    while not q.empty():
        print(q.dequeue(), end='  ')
```

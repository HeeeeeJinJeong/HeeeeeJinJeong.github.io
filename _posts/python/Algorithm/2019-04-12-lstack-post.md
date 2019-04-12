---
title: "lstack"
date: 2019-04-12
categories: algorithm
---

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


class LStack:
    def __init__(self):
        self.top=None

    def empty(self):
        if self.top is None:
            return True
        else:
            return False

    def push(self,data):
        new_node=Node(data)

        new_node.next=self.top
        self.top=new_node
    
    def pop(self):
        if self.empty():
            return None

        cur=self.top
        self.top=self.top.next
        return cur.data
    
    def peek(self):
        if self.empty():
            return None
        
        return self.top.data

if __name__=="__main__":
    s=LStack()

    s.push(1)
    s.push(2)
    s.push(3)
    s.push(4)
    s.push(5)

    while not s.empty():
        print(s.pop(), end=" ")
```

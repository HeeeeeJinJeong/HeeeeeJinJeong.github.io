---
title: "implementation of queue with two stacks"
date: 2019-04-02
categories: stack_queue
---

# implementation of queue with two stacks
 
```python
from stack import Stack

class Queue:
    def __init__(self):
        self.first=Stack()
        self.second=Stack()

    # 첫번째 스택과 두번째 스택이 비었을 경우엔 True, 아니면 False
    def empty(self):
        if self.first.empty() and self.second.empty():
            return True
        else:
            return False

    # wrapping function
    # 큐의 맨 뒤에 데이터를 쌓는다.
    def enqueue(self,data):
        self.first.push(data)

    # 큐 맨 앞의 데이터를 삭제 하면서 반환
    def dequeue(self):
	      # 스택이 비었다면 None 반환
        if self.empty():
            return None
        
	      # 두번째 스택이 비었을 때, 첫번째 스택 데이터를 pop해서 두번째 스택에 push
        if self.second.empty():
            while not self.first.empty():
                self.second.push(self.first.pop())

		    # 두번째 스택에 push한 데이터 반환
        return self.second.pop()
 
 
 	  # 큐 맨 앞 데이터를 반환
    def peek(self):
		    # 스택이 비었다면 None 반환
        if self.empty():
            return None
        
	      # 두번째 스택이 비었을 때, 첫번째 스택 데이터를 pop해서 두번째 스택에 push
        if self.second.empty():
            while not self.first.empty():
                self.second.push(self.first.pop())

	      # 두번째 스택에 push한 데이터 반환
        return self.second.peek()


# test code
if __name__=="__main__":
    q=Queue()

    for i in range(1,6):
        q.enqueue(i)

    while not q.empty():
        print(q.dequeue(), end=' ')
```

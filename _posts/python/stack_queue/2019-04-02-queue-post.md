---
title: "queue"
date: 2019-04-02
categories: stack_queue
---
* queue : First In, First Out (선입선출)

* ADT
1. Q.empty() -> Boolean : 큐가 비어있으면 True, 아니면 False
2. Q.enqueue(data) -> None : 큐의 맨 뒤에 데이터를 쌓는다.
3. Q.dequeue() -> data : 큐 맨 앞의 데이터를 삭제 하면서 반환
4. Q.peek : 큐 맨 앞 데이터를 반환(삭제는 하지 않음)


```python
class Queue:
    def __init__(self):
        self.container=list()
		
	# queue가 비어있으면 True, 아니면 False
    def empty(self):
        if not self.container:
            return Ture
        else:
            return False
	
    # 큐의 맨 뒤에 데이터를 쌓는다.
    def enqueue(self,data):
	    self.container.append(data)
		
    # 큐 맨 앞의 데이터를 삭제 하면서 반환
    def dequeue(self):
        return self.container.pop(0)
		
    # 큐 맨 앞 데이터를 반환(삭제는 하지 않음)
    def peek(self):
        return self.container[0]
		
		
# test code 
if __name__=="__main__":
    q=Queue()
    q.enqueue(1)
    q.enqueue(2)
    q.enqueue(3)
    q.enqueue(4)
    q.enqueue(5)

    q.dequeue()


    while not q.empty():
        print(q.dequeue(), end='  ')

```

---
title: "stack"
date: 2019-04-02
categories: stack_queue
---

* stack : Last In, First Out (후입선출)

* ADT : 추상 자료형
1. 자료구조가 가지고 있는 오퍼레이션(함수)의 나열(목록)
2. 함수 시그니처(인터페이스)만 나열할 뿐, 내부 구현은 표기하지 않는다.(추상화)
3. 함수의 작동방식 설명


* stack ADT
1. S.empty() -> Boolean (스택이 비어있으면 True, 아니면 False)
2. S.push(date) -> None (스택의 맨 위에 데이터를 쌓는다.)
3. S.pop() -> data (스택 맨 위의 데이터를 삭제 하면서 반환)
4. S.peek() -> data (스택 맨 위 데이터를 반환(삭제는 하지 않음))


```python

class Stack:
	def __init__(self):
		self.container=list()
		
	# 리스트가 비어있으면 True, 아니면 False
	def empty(self):
		if not self.container:
			return True
		else:
			return False
	
	# wrapping function
	# 스택의 맨 위에 데이터를 쌓는다.
	def push(self,data):
	self.container.append(data)
	
	# 스택 맨 위의 데이터를 삭제하면서 반환
	def pop(self):
		return self.container.pop()
		
	# 스택 맨 위의 데이터를 반환(삭제는 하지 않음)
	def peek(self):
		return self.container[-1]
		
		
# test code		
if __name__=="__main__":
    s=Stack()

    s.push(1)
    s.push(2)
    s.push(3)
    s.push(4)
    s.push(5)


    while not s.empty():
        print(s.pop(), end='  ')
```

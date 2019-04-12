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
```

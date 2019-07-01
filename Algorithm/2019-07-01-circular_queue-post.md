# circular queue (원형 큐)

1. 크기가 정해져 있는 배열(list)
2. head, tail
3. empty -> head == tail
4. full -> tail+1 == head
5. tail -> 마지막 데이터의 다음을 가리킴

* ADT (Abstract Data Type) : 추상 자료형

```python
class CQueue:
    # ADT
    MAXSIZE=10
    def __init__(self):
        self.__container=[None for _ in range(CQueue.MAXSIZE)]
        self.__head=0
        self.__tail=0

    def is_empty(self):
        if self.__head==self.__tail:
            return True
        return False

    def is_full(self):
        next=self.__step_forward(self.__tail)
        if next==self.__head:
            return True
        return False

    def enqueue(self, data):
        if self.is_full():
            raise Exception("The queue is full")
        self.__container[self.__tail]=data
        self.__tail=self.__step_forward(self.__tail)

    def dequeue(self):
        if self.is_empty():
            raise Exception("The queue is empty")
        ret=self.__container[self.__head]
        self.__head=self.__step_forward(self.__head)
        return ret

    def peek(self):
        if self.is_empty():
            raise Exception("The queue is empty")
        return self.__container[self.__head]

    def __step_forward(self, x):
        x+=1
        if x >= CQueue.MAXSIZE:
            x=0
        return x


if __name__ =="__main__":
    cq=CQueue()

    for i in range(8):
        cq.enqueue(i)

    for i in range(5):
        print(cq.dequeue())

    for i in range(8, 14):
        cq.enqueue(i)

    while not cq.is_empty():
        print(cq.dequeue()) 

    print()
    for i in range(10):
        print(cq._CQueue__container[i])
```

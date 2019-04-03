---
title: "double Linked List"
date: 2019-04-02
categories: Linked_List
---

# double Linked List

* instance member
1. head : 리스트 맨 앞에 있는 더미 
2. tail : 리스트 맨 뒤에 있는 더미
3. d_size : 리스트의 요소 개수


* operations
1. S.empty() -> Boolean : 리스트가 비어있다면 True, 아니면 False
2. S.size() -> integer : 리스트에 있는 요소 개수
3. Insert 계열
  - add_first(data) -> None : 리스트의 맨 앞에 데이터 추가
  - add_last(data) -> None : 리스트의 맨 뒤에 데이터 추가
  - insert_after(data,node) -> None : node 다음에 데이터 추가
  - insert_before(data,node) -> None : node 이전에 데이터 추가
4. Search 계열
  - search_forward(target) -> node : 리스트의 맨 앞 데이터부터 검색
  - search_backward(target) -> node : 리스트의 맨 뒤 데이터부터 검색
5. Delete 계열
  - delete_first() -> None : 리스트의 첫 데이터 삭제
  - delete_last() -> None : 리스트의 마지막 데이터 삭제
  - delete_node(node) -> None : 노드 삭제
  
```python
class Node:
    def __init__(self,data=None):
        self.__data=data
        self.__prev=None
        self.__next=None
        
    # 소멸자 생성(객체가 사라지기 전에 반드시 한 번 호출하는 것을 보장한다.)    
    def __del__(self):
        return self.__data
    
    # data,prev,next의 getter, setter
    @property # data getter
    def data(self):
        return self.__data
        
    @data.setter # data setter
    def data(self,data):
        self.__data=data
    
    
    @property # prev getter
    def prev(self):
        return self.__prev
        
    @prev.setter # prev setter
    def prev(self,p):
        self.__prev=p
        
        
    @property # next getter
    def next(self):
        return self.__next
        
    @next.setter # next.setter
    def next(self,n):
        self.__next=n
        
        
class DoubleLinkedList:
    def __init__(self): 
        # 더미 생성 시점
        # head, tail, d_size
        
        # head와 tail Node()와 연결
        self.head=Node()
        self.tail=Node()
        
        # initialize (초기 설정)
        self.head.next=self.tail
        self.tail.prev=self.head
        self.d_size=0
        
    # 리스트가 비어있다면 True, 아니면 False    
    def empty(self):
        if self.d_size==0:
            return True
        else:
            return False
    
    def size(self):
        return self.d_size
        
        
    # 리스트의 맨 앞에 데이터 추가
    def add_first(self,data):
        # new node 생성
        new_node=Node(data)
        
        # new_node를 더미와 연결
        new_node.next=self.head.next # 새 노드의 next가 첫번째 데이터를 가리키게 한다.
        new_node.prev=self.head # 새 노드의 prev가 head를 가리키게 한다.

        # 리스트 입장에서 연결
        self.head.prev=new_node # head의 다음 노드가 새 노드를 가리키게 한다.
        self.head.next=new_node # head의 next가 새 노드를 가리키게 한다.
            
        self.d_size+=1 # 리스트의 요소 개수 추가
        
    # 리스트의 맨 뒤에 데이터 추가
    def add_last(self,data):
        new_node=Node(data) # 새 노드 생성

        # new_node를 연결한다.
        new_node.prev=self.tail.prev # 새 노드의 prev가 tail앞의 데이터를 가리키게 한다.
        new_node.next=self.tail # 새 노드의 next가 tail을 가리키게 한다.

        # 리스트 입장에서 연결
        self.tail.prev.next=new_node # tail 앞 데이터의 next가 새 노드를 가리키게 한다.
        self.tail.prev=new_node # tail의 prev가 새 노드를 가리키게 한다.

        self.d_size+=1 # 리스트의 요소 개수 추가
        
    # 노드 다음에 데이터 추가
    def insert_after(self,data,node):
        new_node=Node(data) # 새 노드 생성

        # new_node를 연결한다.
        new_node.next=node.next # 새 노드의 next가 노드의 next를 가리키게 한다.
        new_node.prev=node # 새 노드의 prev가 노드를 가리키게 한다.

        # 리스트 입장에서 연결
        node.next.prev=new_node # 노드의 next의 prev가 새 노드를 가리키게 한다.
        node.next=new_node # 노드의 next가 새 노드를 가리키게 한다.

        self.d_size+=1 # 리스트의 요소 개수 추가

    # 노드 이전에 데이터 추가
    def insert_before(self,data,node):
        new_node=Node(data) # 새 노드 생성

        # new_node를 연결한다.
        new_node.prev=node.prev # 새 노드의 prev가 node의 앞 데이터(node.prev)를 가리키게 한다.
        new_node.next=node # 새 노드의 next가 노드를 가리키게 한다.

        # 리스트 입장에서 연결
        node.prev.next=new_node # 노드 앞 데이터의 next(node.prev.next)가 새 노드를 가리키게 한다.
        node.prev=new_node # 노드의 prev가 새 노드를 가리키게 한다.

        self.d_size+=1 # 리스트의 요소 개수 추가
        
    # 리스트의 맨 앞 데이터부터 검색
    def search_forward(self,target):
        cur=self.head.next # cur이 head의 next를 가리킨다.

        # cur이 self.tail에 있는 값 객체와 같지 않으면 반복
        # cur이 self.tail을 만날때까지 반복
        while cur is not self.tail:
            if cur.data==target:
                return cur
            cur=cur.next # target과 다르면 다음으로 넘어감
        return None

    # 리스트의 맨 뒤 데이터부터 검색
    def search_backward(self,target):
        cur=self.tail.prev # cur이 tail의 prev를 가리킨다.

        # cur이 self.head에 있는 값 객체와 같지 않으면 반복
        # cur이 self.head를 만날때까지 반복
        while cur is not self.head:
            if cur.data==target:
                return cur
            cur=cur.prev # target과 다르면 이전으로 넘어감
        return None
        
    # 리스트의 첫 데이터 삭제
    def delete_first(self):
        # 리스트가 비어있을 경우엔 빠져나가기
        if self.empty():
            return 

        self.head.next=self.head.next.next # head의 next가 head 다음 다음 데이터를 가리키게 한다.
        self.head.next.prev=self.head # head 다음 다음 데이터의 prev가 head를 가리키게 한다.

        self.d_size-=1 # 리스트의 요소 개수 삭제
    
    # 리스트의 마지막 데이터 삭제
    def delete_last(self):
        # 리스트가 비어있을 경우엔 빠져나가기
        if self.empty():
            return 

        self.tail.prev=self.tail.prev.prev # tail의 prev가 tail 이전 이전 데이터를 가리키게 한다.
        self.tail.prev.next=self.tail # tail 이전 이전 데이터의 next가 tail을 가리키게 한다.

        self.d_size-=1 # 리스트의 요소 개수 삭제
        
    # 노드 삭제
    def delete_node(self,node):
        node.prev.next=node.next # 노드 앞 데이터의 next가 노드 다음 데이터를 가리키게 한다.
        node.next.prev=node.prev # 노드 뒤 데이터의 prev가 노드 전 데이터를 가리키게 한다.

        self.d_size-=1 # 리스트의 요소 개수 삭제
        
        
        
    # 편의 함수 - generator
    def traverse(self,start=True):
        if start:
            # 리스트의 첫 데이터부터 순회
            cur=self.head.next
            while cur is not self.tail: # cur이 self.tail에 있는 값 객체랑 같은지? 같지 않으면 반복
                yield cur
                cur=cur.next
        else:
            # 리스트의 마지막 데이터부터 순회
            cur=self.tail.prev
            while cur is not self.head:
                yield cur
                cur=cur.prev


# test code
def show_list(dlist):
    # generator 객체
    g=dlist.traverse()

    for node in g:
        print(node.data, end=' ')

    print()

if __name__=="__main__":
    dlist=DoubleLinkedList()

    dlist.add_last(1)
    dlist.add_last(2)
    dlist.add_last(3)
    dlist.add_last(4)
    dlist.add_last(5)

    show_list(dlist)

    searched_data=dlist.search_backward(3)
    if searched_data:
        dlist.insert_after(15, searched_data)
        # print(f'searched_data : {searched_data.data}')
    else:
        print('no such data')

    show_list(dlist)
    dlist.delete_first()
    show_list(dlist)

    del_data=dlist.search_forward(15)
    if del_data:
        dlist.delete_node(del_data)
        del_data=None
    else:
        print('no such data')

    print('*'*100)
```

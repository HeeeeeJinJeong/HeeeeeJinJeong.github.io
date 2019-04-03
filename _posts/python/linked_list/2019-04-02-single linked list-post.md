---
title: "single linked list"
date: 2019-04-02
categories: Linked_List
---

* instance member
1. head : 리스트의 첫번째 노드
2. d_size : 리스트의 요소 개수


* operations
1. S.empty() -> Boolean : 리스트가 비어있다면 True, 아니면 False
2. S.size() -> integer : 리스트에 있는 요소 개수
3. S.add() -> None : 노드를 리스트의 맨 앞에 추가
4. S.search(target) -> node : 리스트에서 target을 찾으면 노드를 반환, 못찾으면 None을 반환
5. S.delete() -> None : 맨 앞 노드를 삭제

```python

class Node:
    def __init__(self, data=None):
        self.data=data
        self.next=None

    # 소멸자 생성 : 객체가 사라지기 전에 반드시 한 번 호출하는것을 보장한다.
    def __del__(self):
        print(f'node[{self.__data}] deleted!')


    # data, link의 getter, setter
    @property # data getter
    def data(self):
        return self.__data

    @data.setter
    def data(self, data):
        self.__data=data

    @property # link getter
    def link(self):
        return self.__link

    @link.setter
    def link(self, l):
        self.__link=l
		
	
class SingleLinkedList:
    def __init__(self):
	# initialize (초기 설정)
	self.head=None
	# 리스트 초기화
	self.d_size=0
	
    # 리스트가 비어있다면 True, 아니면 False
    def empty(self):
        if self.d_size==0:
            return True
        else:
            return False
			
    def size(self):
        return self.d_size
	
    # 노드를 리스트의 맨 앞에 추가
    def add(self, data):
	# 새 노드 생성
        new_node=Node(data)
		
        if self.empty():
            # 새 노드가 head를 가리키게 만든다.
            new_node=self.head
        else:
            # head가 새 노드의 next를 가리키게 만든다.
            self.head=new_node.next	    
        # self.head와 new_node가 같으면 빠져나와서 사이즈 요소 추가
            self.head=new_node
        self.d_size+=1
		
    # 리스트에서 target을 찾으면 노드를 반환, 못찾으면 None을 반환
    def search(self, target):
	# cur이 self.head를 가리킨다.
        cur=self.head
		
        while cur:
            if cur.data==target:
                return cur
            cur=cur.next
        return None
	
    # 맨 앞 노드를 삭제
    def delete(self):
	# 리스트가 비어있을 경우엔 빠져나가기
        if self.empty():
            return
           
        self.head=self.head.next # head를 head.next로 이동하면 head의 reference count가 0이 되면서 함수 종료 후 삭제됨
        self.d_size-=1 # 리스트의 요소 개수 삭제
	
	
    # 편의 함수
    def traverse(self):
        cur=self.head
        while cur:
            yield cur
            cur=cur.next

def show_list(slist):
    print('data size : {}'.format(slist.size()))
    g=slist.traverse()
    for node in g:
        print(node.data, end= '  ')
    print()

if __name__=="__main__":
    print('*'*100)
    slist=SingleLinkedList()

    print('데이터 삽입')
    slist.add(3)
    slist.add(1)
    slist.add(5)
    slist.add(2)
    slist.add(7)
    slist.add(8)
    slist.add(3)
    show_list(slist)

    print('데이터 탐색')
    target=5
    res=slist.search(target)
    if res:
        print('데이터 {} 검색 성공'.format(res.data))
    else:
        print('데이터 {} 탐색 실패'.format(target))
    res=None
    print()

    print('데이터 삭제')
    slist.delete()
    slist.delete()
    slist.delete()
    show_list(slist)
    
    print('*'*100)

```

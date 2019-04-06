---
title: "person"
date: 2019-04-06
categories: oop
---

```python
class Person:
    def __init__(self,name,money):
        self.name=name
        self.money=money
        
    def give_money(self,other,money):
        self.money-=money
        other.get_money(money)
        
#---------------------------------------------------------
    # def get_money(self,money):
        # self.money+=money

    @property # getter
    def _money(self,money):
        self.money+=money


    # def set_money(self,money):
        # self.money=money

    @_money.setter # setter
    def _money(self,money):
        self.money=money
#---------------------------------------------------------
    def show(self):
        print(f'{self.name} : {self.money}')
    
    
# test code
if __name__ == "__main__":
    # 객체의 멤버에 접근(getter)이나 수정(setter)을 할 때는 반드시 메서드(access function)를 이용한다.
    j=Person('john',3000)
    m=Person('mark',100)
    j.show() # john : 3000
    m.show() # mark : 100

    j.get_money(2000)
    j.show() # john : 5000

    j.give_money(m,1000)

    j.show() # john : 4000
    m.show() # mark : 1100
```

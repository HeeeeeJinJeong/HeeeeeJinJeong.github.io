---
title: "abstract class"
date: 2019-04-06
categories: class
---

# abstract class (추상 클래스)
* 인스턴스를 만들 수 없다.
* 반드시 하나 이상 존재
* abstract method(추상 메서드)
  1. pure virtual method : 몸체(body)가 없는 함수 (pass)
  2. 파생 클래스에서 반드시 overriding 해야 함
  
```python
from abc import *

# abstract code
# 메서드 구현부를 pass로 해 함수 몸체를 비워두면 eat()는 추상 메서드가 됨
# 파생 클래스에서 반드시 overriding 해야 함(재정의 필수)
class Animal:
    @abstractmethod
    def eat(self):
        pass

class Lion(Animal):
    def eat(self):
        print('eat meat')

class Deer(Animal):
    def eat(self):
        print('eat grass')

class Human(Animal):
    def eat(self):
        print('eat meat and grass')


# test code
if __name__=='__main__':
    animals=[]
    animals.extend((Lion(), Deer(), Human()))
    
    for animal in animals:
        animal.eat() # polymorphism code
```

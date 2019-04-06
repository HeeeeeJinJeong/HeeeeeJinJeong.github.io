---
title: "method overriding"
date: 2019-04-06
categories: class
---

# method overriding
파생클래스(자식)에서 상속받은 메서드를 다시 구현하는 것
(비슷한 기능을 자식에 맞춰서 메서드를 재정의하는 것)

* overrides(replaces) a method provided by base class
* same name, args
* execution : by the object

```python
class CarOwner:
    def __init__(self, name):
        self.name=name
        
    def concentrate(self):
        print(f'{self.name} can not do anything else')
        
class Car:
    def __init__(self,owner_name):
        self.owner=CarOwner(owner_name)
        
    def drive(self):
        self.owner.concentrate()
        print(f'{self.owner.name} is driving now')
        
        
# Car 클래스를 상속하되, 어울리지 않는 drive() 메서드만 클래스 안에서 재정의
class SelfDrivingCar(car):
    # Method overriding
    def drive(self):
        print('driving by itself')
        
   
# test code
if __name__=='__main__':
    car=Car('mark')
    car.drive()
    # mark can not do anything else
    # mark is driving now
    
    car2=SelfDrivingCar('yang')
    car2.drive()
    # driving by itself
```

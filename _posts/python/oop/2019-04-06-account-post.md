---
title: "account"
date: 2019-04-06
categories: oop
---

```python3
class Account:
    # 클래스 멤버(class member)
    # 모든 객체가 공유한다.(공통으로 적용해야 할 사항)
    # 전역변수(global variable)를 대체한다.
    # ex) 이자율
    interest_rate=0.08
    num_of_account=0

    # 클래스 메서드(class method)
    # 객체가 하나도 없는 상태에서도 호출이 가능(if __name__=='__main__': 에 내용이 없어도 Account.num_of_account 로 사용가능)
    # 전역함수를 대체
    # 전역함수를 대체할 때는 static method를 쓸 수도 있다.
    # ** 대체 생성자(alternative constructor) 
    # my_acnt.num_of_account 도 가능(객체를 통해서도 접근 가능)

    def __init__(self,clnt_name,clnt_balance):
        # Instance member
        self.clnt_name=clnt_name
        self.balance=clnt_balance
        
        # 정보은닉(앞에 언더바 두개 붙이기)
        # self.__balance=clnt_balance
        # _클래스명__으로 접근 가능 self._Account__balance=clnt_balance
        
        # 클래스 멤버에 접근하는 방법
        Account.num_of_account+=1
        
    # Instance method
    def deposit(self, money): # 입금
        """
        a.deposit(money) -> boolean
        meney > 0 입금 성공, 아니면 에러메시지 출력 후 실패
        """
        
        if money<0:
            print('입금은 0원 초과부터 가능합니다.')
            return False
            
        self.balance+=money
        return True
        
    # Instance method
    def withdraw(self, money): # 출금
        """
        a.withdraw(money) -> integer
        return : 인출된 돈
        잔고가 모자라면 None
        """
        if self.balance < money:
            print('출금하려는 돈이 부족합니다.')
            return None
        else:
            self.balance-=money
            return money
    
    # Instance method
    def transfer(self, other, money): # 계좌이체
        # 내가 가진 돈
        self.balance-=money
        # message passing
        # 다른 객체의 상태 정보(인스턴스 멤버)를 변경할 때는 반드시 상대 객체가 가진 메서드를 이용
        other.deposit(money)
        
    def __str__(self): # 원하는 출력 포맷
        return f'{self.clnt_name} : {self.balance}'
        


# test code
if __name__=='__main__':
    # 객체를 생성
    my_acnt=Account('greg', 5000)
    your_acnt=Account('john', 2000)

    # Instance method 호출하는 방법
    my_acnt.deposit(7000)
    

    # method : 입력 + 인스턴스 멤버(상태정보, 데이터)에 의해 결과 값이 결정, 인스턴스 멤버(상태정보)를 바꾸는 역할
    # function : 입력에 의해서 출력이 결정
    res1=my_acnt.withdraw(3000)
    

    # 객체간의 상호작용 -> 객체가 가지고 있는 멤버 값(상태정보, 데이터)
    # interaction by method(상호작용은 메서드에 의해서)
    # my_acnt.transfer(your_acnt, 1000)
    my_acnt.balance-=1000
    your_acnt.balance+=1000
    print(my_acnt, your_acnt)

    # 전역함수 대용
    # print(Account.func(3,4))
    # print(type(Account.func)) # <class 'function'>
    print(type(Account.get_num_of_account)) # <class 'method'>
    print(type(my_acnt.deposit)) # <class 'method'>

    # 대체 생성자를 이용한 객체의 생성
    s='james_6000'
    his_acnt=Account.string_constructor(s)
    print(his_acnt)
```

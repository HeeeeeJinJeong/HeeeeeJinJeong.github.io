---
title: "reduce"
data: 2019-04-03
categories: lambda
---

# reduce
자료구조(list, tuple)를 연산을 통해서 단 하나의 값으로 만드는 함수
- from functools import reduce 필요

```python
from functools import reduce

li=[2, 3, -5, 6, -2, 1, -10]
result=reduce(lambda a,b: a+b, li, 100) # 100은 초기값, 기입 안하면 자동으로 0
print(result) # 95


# 문자 수 세기
# dic={'a':4, 'b':3, 'c':1}
# 1. 파이썬 함수는 식 -> 값을 반환
# 2. 논리연산을 할 때 마지막에 참조한 값을 반환
li=['a','b','a','b','b','a','c','a']
res=reduce(lambda dic, ch:dic.update({ch:dic.get(ch,0)+1}) or dic, li, {})
print(res) # {'a': 4, 'b': 3, 'c': 1}


dic={'a':1,'b':2}
dic.get('c',0) # dic에 'c'가 있으면 'c'의 값을 반환, 없으면 0 반환
print(dic.get('c',0)) # 0

dic.update({'a':3,'c':4}) # dic에 해당값이 있으면 갱신, 없으면 추가
print(dic) # {'a': 3, 'b': 2, 'c': 4}
```

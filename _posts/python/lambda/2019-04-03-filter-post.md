---
title: "filter"
data: 2019-04-03
categories: lambda
---

# filter
lazy evaluation(게으른 연산) : 함수의 실행 시기를 내가 결정함, 원할때만 결과값을 가져옴

```python
li=[-3,5,-1,2,-5,-4,14]
f=filter(lambda e: e>0,li) # 0보다 큰 데이터만 필터링

for i in f:
    print(i,end=' ') # 5,2,14
```

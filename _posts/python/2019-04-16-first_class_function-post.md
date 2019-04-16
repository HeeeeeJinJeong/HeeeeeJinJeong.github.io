---
title: "first class function"
date: 2019-04-16
categories: python
---

# first class function
1. argument(parameter)로 사용할 수 있는가
2. variable(변수) 로 사용할 수 있는가
3. return 할 수 있는가

```python
def f(a,b):
    return a+b
    
def g(func,c,d):
    return func(c,d)
    
# 1. argument(parameter)로 사용할 수 있는가
a=10
b=20
print(g(f,a,b)) # 30

# 2. variable(변수) 로 사용할 수 있는가
f_var=f
print(f_var(20,30)) # 50

# 3. return 할 수 있는가
def create_calc(kind):
    if kind=='add':
        def add(a,b):
            return a+b
        return add
    elif kind=='sub':
        def sub(a,b):
            return a-b
        return sub
        
f=create_calc('add')
f2=create_calc('sub')
print(f(10,20)) # 30
print(f2(10,20)) # -10
```

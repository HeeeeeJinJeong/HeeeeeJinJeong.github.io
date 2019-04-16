---
title: "function"
date: 2019-04-16
categories: python
---

```python
# 전역변수 : global variable
a=10
b=20
def func(c,d):

    # 지역변수 : local variable
    e=c+d
    return e

print(func(a,b)) # 30


g_var=20
def func(val):
    global g_var
    g_var+=30
    
func(g_var)
print(g_var) # 50
```

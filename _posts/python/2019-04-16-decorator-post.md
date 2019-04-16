---
title: "decorator"
date: 2019-04-12
categories: python
---

# decorator
쉽게 기능을 추가할 수 있다.

```python
def outer(org_func):
    def inner(*args, **kwargs): # *, ** -> 가변인자
        # 추가하는 기능을 구현
        print('added functionalty')
        return org_func(*args, **kwargs) # *,** unpacking / org_func 함수의 실행값 반환
    return inner
    
@outer
def func(a,b):
    return a+b
    
# added functionalty
# 11
print(func(3,2))


# ex 2
from functools import wraps
import time

def benchmarker(org_func):
    @wraps(org_func)
    def inner(*args, **kwargs):
        start=time.time()
        result=org_func(*args, **kwargs)
        elapsed=time.time()-start

        print(f'elapsed time : {elapsed:.2f}')
        return result
    return inner


@benchmarker
def something(a,b):
    time.sleep(5)
    return a+b

print(something(1,2))
```

---
title: "lambda"
data: 2019-04-03
categories: lambda
---

# lambda
- 이름이 없는 함수(익명함수)
- return을 명시하지 않더라도 무조건 값을 반환

```python
li=[1,2,3,4,5,6,7,8,9,10]
li.sort(key=lambda x: x%2==0) # 정렬하는데 필요한 key인자에 람다함수를 전달(x%2==0이 True면 오름차순이므로 뒤에 정렬됨)
print(li) # [1, 3, 5, 7, 9, 2, 4, 6, 8, 10]


# 함수처럼 사용
# return을 명시하지 않더라도 무조건 값을 반환
f2=lambda a,b: a+b
print(f2(2,3))
```

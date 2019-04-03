---
title: "bubble sort"
date: 2019-04-03
categories: algorithm
---

# bubble sort
맨 처음에 위치한 데이터와 바로 뒤에 있는 데이터부터 순서대로 두 데이터씩 비교, 앞에 위치한 데이터가 뒤에 위치한 데이터보다 크면 두 데이터를 교체
(정렬된 데이터는 비교하지 않으므로 비교 횟수는 반복을 할 때마다 줄어듦, 데이터 개수가 n일 때 총 n-1회 반복하고 반복할 때마다 1회씩 줄어듦)

```python
def bubble_sort(data):
    data_len=len(data) # data_len은 data의 길이
    
    for i in range(data_len-1): # 데이터 개수가 n일 때 총 n-1회 반복
        for j in range(data_len-1-i): # 반복할 때마다 1회씩 줄어듦
            if data[j]>data[j+1]: # j가 j+1보다 크면 데이터 교체
                data[j],data[j+1]=data[j+1],data[j]
           
           
# test code
if __name__=="__main__":
    li=[2,3,5,2,7,1,9,4]
    bubble_sort(li)
    print(li)
```

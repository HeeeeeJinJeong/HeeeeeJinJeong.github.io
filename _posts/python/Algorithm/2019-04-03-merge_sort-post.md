---
title: "merge sort"
date: 2019-04-02
categories: algorithm
---

# merge sort
데이터가 1개가 될 때까지 쪼갠 후 각각 비교해서 앞에 위치한 데이터가 클 경우 교체

```python
import random

def merge(data,start,mid,end):
    left_idx=start
    right_idx=mid+1
    temp=[]
    
    while left_idx<=mid and right_idx<=end: # left_idx가 mid보다 작거나 같고, right_idx가 end보다 작거나 같으면 반복
        if data[left_idx] < data[right_idx]: # left_idx와 right_idx를 비교했을 때 left_idx가 더 작으면 빈 리스트에 추가
            temp.append(data[left_idx])
            left_idx+=1 # 다음 데이터 비교를 위해 한 칸 이동
        else:
            temp.append(data[right_idx]) # 비교 후 남은 right_idx를 빈 리스트에 추가 후 right_idx 한 칸 이동
            right_idx+=1
          
          
    while left_idx <= mid: 
        temp.append(data[left_idx])
        left_idx+=1 # 다음 데이터 비교를 위해 한 칸 이동
        
        
    while right_idx <= end:
        temp.append(data[right_idx])
        right_idx+=1
        
    data[start:end+1]=temp
    

# 재귀
def merge_sort(data,start,end):
    # 기저 조건
    if start >= end:
        return
        
    mid=(start+end)//2
    merge_sort(data,start,mid)
    merge_sort(data,mid+1,end)

    merge(data,start,mid,end)
    
    
# test code
if __name__=="__main__":
    while True:
        num_data=int(input('데이터 개수(종료:0):'))
        if not num_data:
            break
            
        data=[random.randint(1, 100) for _ in range(num_data)]
        print(data)
        merge_sort(data, 0, len(data)-1)
        print(data)
```

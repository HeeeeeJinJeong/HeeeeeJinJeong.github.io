# quick sort (빠른정렬)

partition : pivot을 기준으로 왼쪽은 pivot보다 작은 데이터, 오른쪽은 큰 데이터가 모이도록 분할해서 정렬하는 것(재귀함수 이용)

* start : 첫번째 인덱스
* end : 마지막 인덱스
* pivot : 정렬되지 않은 리스트에서 가운데 위치한 인덱스의 데이터

1. 재귀함수의 기저조건은 start가 end보다 크거나 같을 때
2. start는 pivot보다 큰 데이터를 만나면 멈춤
3. end는 pivot보다 작은 데이터를 만나면 멈춤
4. 멈춘 두 데이터를 교체 후 start는 한 칸 +, end는 한 칸 -

```python
def quick_sort(arr,start,end):
    # 기저조건 : start가 end보다 크거나 같을 때
    if start>=end:
        return
        
    left=start
    right=end
    pivot=arr[(left+right)//2] # 정렬되지 않은 리스트에서 가운데 위치한 인덱스의 '데이터'
    
    # partition
    while left<=right: # left와 right가 교차할 때까지 반복
        while arr[left]<pivot: # pivot보다 큰 데이터를 만날때까지 +
            left+=1
        while arr[right]>pivot: # pivot보다 작은 데이터를 만날때까지 -
            right-=1
            
        if left<=right: # left와 right가 교차하지 않았다면 두 데이터를 교환 후 left는 +, right는 -
            arr[left],arr[right]=arr[right],arr[left]
            left+=1
            right-=1
            
    # 재귀 quick_sort(arr,start,end) 재설정
    quick_sort(arr,start,right)
    quick_sort(arr,left,end)
    
if __name__=='__main__':
    arr=[2,5,4,1,8,10,5,3,6,7,9,11]
    quick_sort(arr, 0, len(arr)-1)
    print(arr)
```

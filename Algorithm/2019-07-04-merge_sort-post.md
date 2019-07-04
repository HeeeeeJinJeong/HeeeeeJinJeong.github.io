# merge sort (병합정렬)
데이터가 1개가 될 때까지 쪼갠 후 각각 비교해서 앞에 위치한 데이터가 클 경우 교체

1. left가 mid보다 작거나 같고, right가 end보다 작거나 같으면 반복
2. left와 right를 비교했을 때 left가 더 작으면 빈 리스트에 추가
3. 다음 데이터 비교를 위해 한 칸 이동
4. 비교 후 남은 right를 빈 리스트에 추가 후 right 한 칸 이동
5. 다음 데이터 비교를 위해 한 칸 이동

```python
def merge(arr, start, mid, end):
    left = start
    right = mid+1
    temp = []

    # left가 mid보다 작거나 같고, right가 end보다 작거나 같으면 반복
    while left <= mid and right <= end:
        # left와 right를 비교
        if arr[left] < arr[right]:
            # left가 더 작으면 temp에 추가
            temp.append(arr[left])
            # 추가 후 이동
            left += 1
        else:
            # temp에 남아있는 right 추가
            temp.append(arr[right])
            # 추가 후 이동
            right +=1

    # left가 mid보다 작거나 같다면
    while left <= mid:
        # temp에 left추가
        temp.append(arr[left])
        # 추가 후 이동
        left += 1

    # right가 end보다 작거나 같다면
    while right <= end:   
        # temp에 right추가
        temp.append(arr[right])
        # 추가 후 이동
        right +=1

    # arr에 temp update
    arr[start:end+1] = temp

def merge_sort(arr, start, end):
    # base case
    if start >= end:
        return

    mid = (start+end)//2
    # divide
    merge_sort(arr, start, mid)
    merge_sort(arr, mid+1, end)

    # conquer
    merge(arr, start, mid, end)

if __name__=='__main__':
    arr=[2,5,4,1,8,10,5,3,6,7,9,11]
    merge_sort(arr, 0, len(arr)-1)
    print(arr)
```

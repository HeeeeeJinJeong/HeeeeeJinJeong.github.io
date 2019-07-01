# selection sort (선택정렬)

1. i가 0번 인덱스부터 시작
2. j는 1번 인덱스(i+1)부터 시작
3. j와 min_index를 비교해서 j가 작으면 min_index을 j값으로 업데이트
4. 후에 for문을 빠져나와서 i와 min_index를 스와핑

```python
def selection_sort(arr):
    n=len(arr)

    for i in range(n-1):
        min_index = i

        for j in range(i+1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]
        
if __name__ =="__main__":
    arr=[5,2,9,3,13,6]
    selection_sort(arr)
    print(arr)
```

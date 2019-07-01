# bubble sort (거품정렬)

1. 맨 처음에 위치한 데이터와 바로 뒤에 있는 데이터부터 순서대로 두 데이터씩 비교
2. 앞에 위치한 데이터가 뒤에 위치한 데이터보다 크면 두 데이터를 교체
3. 정렬된 데이터는 비교하지 않으므로 비교 횟수는 반복을 할 때마다 줄어듦
4. 데이터 개수가 n일 때 총 n-1회 반복하고 반복할 때마다 1회씩 줄어듦

```python
def bubble_sort(arr):
    n = len(arr)

    for i in range(n-1):
        for j in range(n-1-i):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

if __name__ =="__main__":
    arr=[5,2,9,3,13,6]
    bubble_sort(arr)
    print(arr)
```

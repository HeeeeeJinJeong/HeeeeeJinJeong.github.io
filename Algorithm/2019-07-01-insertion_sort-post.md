# insertion sort (삽입정렬)


* for + while 문
```python
def insertion_sort(arr):
    n = len(arr)

    for i in range(1, n):
        temp = arr[i]
        j = i-1
        while j != -1:
            if arr[j] > temp:
                arr[j+1] = arr[j]
                j -= 1
            else:
                break
        arr[j+1] = temp

if __name__ =="__main__":
    arr=[5,2,9,3,13,6]
    insertion_sort(arr)
    print(arr)
```

* for문
```python
def insertion_sort(arr):
    n=len(arr)
    temp=None
    for i in range(1, n):
        temp=arr[i]
        for j in range(i-1, -2, -1):
            if j == -1:
                break
            if arr[j] > temp:
                arr[j+1]=arr[j]
            else:
                break
        arr[j+1]=temp
        
if __name__ =="__main__":
    arr=[5,2,9,3,13,6]
    insertion_sort(arr)
    print(arr)
```

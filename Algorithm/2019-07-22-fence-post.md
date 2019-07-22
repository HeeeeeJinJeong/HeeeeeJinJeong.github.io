# 울타리 잘라내기
- 너비가 1인 판자를 붙인 울타리가 있다.
- 판자의 높이가 주어지고 울타리의 일부를 잘라내려고 할 때
- 가장 넓이가 넓게 잘라내는 프로그램을 작성하시오

```python
N = 7
height = [7, 1, 5, 9, 6, 7, 3]

def exhaustive():
    ret = 0
    for left in range(N):
        h = height[left]

        for right in range(left, N):
            h = min(h, height[right])
            temp = (right - left + 1) * h
            ret = max(ret, temp)
    return ret

# print(exhaustive()) # 20

# --------------------divide_and_conquer--------------------

def divide_and_conquer(left, right):
    # base case
    if left == right:
        return height[left]

    # max of A --> [left]
    mid = (left+right) // 2
    left_ret = divide_and_conquer(left, mid)

    # max of A --> [right]
    right_ret = divide_and_conquer(mid+1, right)
    ret = max(left_ret, right_ret)

    low = mid
    high = mid+1
    h = min(height[low], height[high])
    ret = max(ret, h*2)

    while low > left or high < right:
        if low > left and (high == right or height[low-1] > height[high+1]):
            low -= 1
            h = min(h, height[low])
        else:
            high += 1
            h = min(h, height[high])

        ret = max(ret, h * (high - low + 1))

    return ret

print(divide_and_conquer(0, N-1)) # 20
```

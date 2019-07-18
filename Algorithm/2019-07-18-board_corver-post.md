# board cover

HxW의 보드가 검은색과 흰색으로 채워져 있다.<br>
모든 흰칸을 L자 모양의 흰색 블록으로 덮고 싶다.<br>
블록은 회전 가능하지만, 겹치거나 검은색을 침범하거나 밖으로 나가서는 안된다.<br>
보드가 있을 때 이를 덮는 방법의 수를 계산하는 프로그램을 만드시오

```python
H = 8
W = 10
board = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
]

'''
case 1 : [[1, 0], [0, 1], [0, 0]]
0 0
0

case 2 : [[0, 1], [1, 1], [0, 0]]
0 0
  0

case 3 : [[1, 0], [1, 1], [0, 0]]
0
0 0

case 4 : [[1, 0], [1, -1], [0, 0]]
  0
0 0
'''
cases = [
    [[1, 0], [0, 1], [0, 0]],
    [[0, 1], [1, 1], [0, 0]],
    [[1, 0], [1, 1], [0, 0]],
    [[1, 0], [1, -1], [0, 0]],
]

# [y, x] 위치가 보드를 벗어났는지 판단하는 함수
def in_range(y, x):
    if y < 0 or x < 0 or y <= H or x <= W:
        return True
    return False

# [y, x] 에 c타입의 블록을 넣는 함수
def put(y, x, c):
    # c : 블록 타입
    ret = True
    for point in c:
        _y = y + point[0]
        _x = x + point[1]
        if in_range(_y, _x):
            ret = False
        else:
            board[_y][_x] += 1
            if board[_y][_x] > 1: # 벽이 있다면
                ret = False
    return ret

# [y, x] 에서 c타입의 블록을 빼는 함수
def get(y, x, c):
    for point in c:
        _y = y + point[0]
        _x = x + point[1]
        if not in_range(_y, _x):
            continue
        else:
            board[_y][_x] -= 1

def solve():
    # base case
    fx = fy = None
    for i in range(H):
        for j in range(W):
            if board[i][j] == 0:
                fx = j
                fy = i
                break
        if fx != None:
            break

    if fx == None:
        return 1

    ret = 0
    for c in cases:
        if put(fy, fx, c):
            ret += solve()
        get(fy, fx, c)
    return ret

if __name__ == '__main__':
    print(solve())
```

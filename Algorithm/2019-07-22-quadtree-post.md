# 쿼드트리 뒤집기
- 모든 픽셀이 블랙인 경우 : b
- 모든 픽셀이 화이트인 경우 : w
- 모든 픽셀이 같은 색이 아니면 2x2로 4등분하여 x(0,0)(0,1)(1,0)(1,1)로 압축한다.
- 압축된 문자열이 주어질 때 이를 뒤집은 압축 문자열을 생성하는 프로그램을 만드시오

```python
n = 16
compressed = 'xxwwwbxwxwbbbwwxxxwwbbbwwwwbb'

class Iterator:
    def __init__(self, raw_string):
        self.string = raw_string
        self.iterator = -1

    def __iter__(self):
        return self
    
    def __next__(self):
        self.iterator += 1
        if self.iterator == len(self.string):
            raise StopIteration
        return self.string[self.iterator]

it = Iterator(compressed)
# for ch in it:
#     print(ch, end=' ')

ret = [[None for _ in range(n)] for _ in range(n)]
def decompress(it, y, x, size):
    # base case
    first = next(it)
    # recursion
    if first == 'b' or first == 'w':
        for i in range(size):
            for j in range(size):
                ret[y+i][x+j] = first

    # decompress 호출
    # first = 'x'
    else:
        new_size = size//2
        decompress(it, y, x, new_size)
        decompress(it, y, x+new_size, new_size)
        decompress(it, y+new_size, x, new_size)
        decompress(it, y+new_size, x+new_size, new_size)

decompress(Iterator(compressed), 0, 0, n)

for row in ret:
    for c in row:
        if c == 'b':
            print('#', end='')
        else:
            print('-', end='')
    print()
    
'''
--------------##
--------------##
------------####
------------####
----####--------
----####--------
----####--------
----####--------
--##----########
####----########
--------########
--------########
----############
----############
----############
----############
'''

def solve(it):
    head = next(it)

    if head == 'b' or head == 'w':
        return head

    # head == 'x'
    else:
        c1 = solve(it)
        c2 = solve(it)
        c3 = solve(it)
        c4 = solve(it)
    
    return 'x' + c3 + c4 + c1 + c2

print(solve(Iterator(compressed))) # xxwbxwwxbbwwbwbxwbwwxwwwxbbwb
```

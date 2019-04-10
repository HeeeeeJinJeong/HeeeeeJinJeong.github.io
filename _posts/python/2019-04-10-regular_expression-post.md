
---
title: "regular_expression"
date: 2019-04-06
categories: python
---


# regular_expression 정규 표현식

```python
import re

# method
# match() 문자열의 처음부터 정규식과 매치되는지 조사한다.
# search() 문자열 전체를 검색하여 정규식과 매치되는지 조사한다.
# findall() 정규식과 매치되는 모든 문자열(substring)을 리스트로 리턴한다
# group() 매칭된 전체 문자열 반환
# start() 매칭된 문자열의 시작 인덱스 반환
# end() 매칭된 문자열의 마지막 인덱스 반환
# span() 매칭된 문자열의 시작과 마지막 인덱스를 튜플로 반환

# \\ : \
# \d : [0-9]
# \D : [^0-9]
# \s : white space (탭,엔터,스페이스 바…)
# \S : not white space
# \w : [a-zA-Z0-9]
# \W : [^a-zA-Z0-9]

re.search(r'\d+', 'abc 123 bbb') # <re.Match object; span=(4, 7), match='123'>


# * 0부터 무한대, 찾는 데이터가 없어도 매칭됨
re.match(r'a*', 'bc') # <re.Match object; span=(0, 0), match=''>
re.search(r'[abc]*', '123') # <re.Match object; span=(0, 0), match=''>


# + 1부터 무한대
re.search(r'[abc]+','abc123') # <re.Match object; span=(0, 3), match='abc'>


# ? 있거나, 없거나
re.match(r'a?', 'abc') # <re.Match object; span=(0, 1), match='a'>
re.match(r'a?', 'bc') # <re.Match object; span=(0, 0), match=''>


# {n} 반드시 n번 반복되어 매치
re.search(r'a{4}','aaaabb') # <re.Match object; span=(0, 4), match='aaaa'>


# {m,n} m부터 n까지 반복
re.search(r'a{3,10}','aaaaaaaaaaaaaaaaaabb') # <re.Match object; span=(0, 10), match='aaaaaaaaaa'>


# ^
# 1. 문자열의 시작에 데이터가 있으면 반환
# 2. [^abc] 의 형식으로 쓸 경우, 문자열의 시작에 a나 b나 c가 아닌 문자열 반환
re.search(r'^abc','abcdefabcxyz') # <re.Match object; span=(0, 3), match='abc'>
re.search(r'[^abc]','abdcefabcxyz') # <re.Match object; span=(2, 3), match='d'>


# $ 문자열의 마지막
re.search(r'abc$','defabcxyzabc') # <re.Match object; span=(9, 12), match='abc'>


# row string
re.search('\\\\','dir1\dir2') # <re.Match object; span=(4, 5), match='\\'>
re.search(r'\\','dir1\dir2') # <re.Match object; span=(4, 5), match='\\'>


# group 괄호로 감싸서 구분함
pattern=r'(\d{3})[-./ ]*(\d{3,4})[-./ ]*(\d{4})'
string='010-1111-2222'
m=re.search(pattern,string)
m # <re.Match object; span=(0, 13), match='010-1111-2222'>
m.group() # '010-1111-2222'
m.group(1) # '010'
m.group(2) # '1111'
m.group(3) # '2222'

pattern2=r'(a(b)c)'
string='abcde'
m=re.search(pattern2,string)
m # <re.Match object; span=(0, 3), match='abc'>
m.group(1) # ’abc'
m.group(2) # ‘b’


# re.compile
string='abcde'
re.compile(pattern2) # re.compile('(a(b)c)')
p=re.compile(pattern2)
p.match(string) # <re.Match object; span=(0, 3), match='abc'>


# findall()
string='abcde'
p=re.compile(r'(a(b)c)')
p.findall(string) # [('abc', 'b')]


# ex)
# 1. phone number
cell_li=['010-1111-2222','01022223333','010.333.4444','010 4444 5555']
pattern=r'\d{3}[-./ ]*\d{3,4}[-./ ]*\d{4}'
for cell in cell_li:
	m=re.search(pattern, cell)
	if m:
		print(m.group())

# 010-1111-2222
# 01022223333
# 010.333.4444
# 010 4444 5555

# 2. ‘a_1 = 100'
pattern=r'([a-zA-A_]\w*)\s*=\s*([0-9]+)'
p=re.compile(pattern)
p # re.compile('([a-zA-A_]\\w*)\\s*=\\s*([0-9]+)')
m=p.search('a_1 = 100')
m # <re.Match object; span=(0, 9), match='a_1 = 100'>
m.group() # ‘a_1 = 100'
m.group(1) # ‘a_1’
m.group(2) # ’100'

# 3. email
pattern=r'([a-zA-Z0-9_]+)@([a-zA-Z0-9]+)\.([a-zA-Z0-9]+)'
string="my email address is abcde@gmail.com"
re.search(pattern,string) # <re.Match object; span=(20, 35), match='abcde@gmail.com'>
```

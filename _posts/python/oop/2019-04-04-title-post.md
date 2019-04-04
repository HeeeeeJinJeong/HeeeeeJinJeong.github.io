---
title: ""
date: 2019-04-04
categories: oop
---

1. 절차 지향 프로그래밍(procedural programming)
```python
from openpyxl import load_workbook
form functools import reduce
import math

raw_data={}
wb=load_workbook('exam.xlsx')
ws=wb.active
g=ws.rows

for name, score in g:
    raw_data[name.value]=score.value
    
    
scores=list(raw_data.values())
s=0

for score in scores:
    s+=score
    
print(scores) # [95, 25, 50, 15, 100, 10, 25, 80, 95, 20]


# 학생 평균 average
avrg=round(s/len(scores),1)

s=0
for score in scores:
    s+=(score-avrg)**2

# 학생 점수 분산 variance
variance=round(s/len(scores),1)

# 표준 편차 : std_dev
std_dev=round(math.sqrt(variance),1)

print(f'평균 : {avrg}, 분산 : {variance}, 표준편차 : {std_dev}') # 평균 : 51.5, 분산 : 1240.2, 표준편차 : 35.2

# 성적은 평균 이상이지만 학생들의 실력 차이가 크다. 주의 요망!
if avrg < 50 and std_dev > 20:
    print('성적이 너무 저조하고 학생들의 실력 차이가 너무 크다.')
elif avrg > 50 and std_dev > 20:
    print('성적은 평균 이상이지만 학생들의 실력 차이가 크다. 주의 요망!')
elif avrg < 50 and std_dev < 20:
    print('학생들의 실력 차이는 크지 않지만 성적이 너무 저조하다. 주의 요망!')
elif avrg > 50 and std_dev < 20:
    print('성적도 평균 이상이고 학생들의 실력 차이도 크지 않다.')

#-------------------------------------------------------------------------------------------------------

raw_data={}
wb=load_workbook('exam.xlsx')
ws=wb.active
g=ws.rows

for name_cell, score_cell in g:
    raw_data[name_cell.value]=score_cell.value

print(raw_data)

# 학생 평균 average
scores=tuple(raw_data.values())
avrg=reduce(lambda a,b: a+b, scores)/len(scores)
print(avg)

# 학생 점수 분산 variance
var=reduce(lambda res, x: (avg-x)**2+res, scores, 0)/len(scores)
print(var)

# 표준 편차 : std_dev
std_dev=math.sqrt(var)

print("평균:{}, 분산:{}, 표준편차:{}".format(avg, var, std_dev))

if avrg < 50 and std_dev > 20:
    print('성적이 너무 저조하고 학생들의 실력 차이가 너무 크다.')
elif avrg > 50 and std_dev > 20:
    print('성적은 평균 이상이지만 학생들의 실력 차이가 크다. 주의 요망!')
elif avrg < 50 and std_dev < 20:
    print('학생들의 실력 차이는 크지 않지만 성적이 너무 저조하다. 주의 요망!')
elif avrg > 50 and std_dev < 20:
    print('성적도 평균 이상이고 학생들의 실력 차이도 크지 않다.')
```

2. 객체 지향 프로그래밍(object-oriented programming)
```python
from openpyxl import load_workbook
from functools import reduce
import math


# raw_data={}
# wb=load_workbook('exam.xlsx')
# ws=wb.active
# g=ws.rows

# for name_cell, score_cell in g:
#     raw_data[name_cell.value]=score_cell.value

def get_raw_data(filename):
    raw_data={}
    wb=load_workbook(filename)
    ws=wb.active
    g=ws.rows
    
    for name_cell, score_cell in g:
        raw_data[name_cell.value]=score_cell.value
        
    return raw_data


# 점수들을 튜플형식으로
# scores=tuple(raw_data.values())
def get_scores(raw_data):
    return tuple(raw_data.values())


# 학생 평균 average
# avg=reduce(lambda a,b: a+b, scores)/len(scores)
def get_average(score):
    avg=reduce(lambda a,b: a+b,score)/len(score)
    return round(avg,2)


# 학생 점수 분산 variance
# var=reduce(lambda res, x: (avg-x)**2+res, scores, 0)/len(scores)
def get_variance(scores,avg):
    var=reduce(lambda res, x:(avg-x)**2+res, scores, 0)/len(scores)
    return round(var,2)
    
    
# 표준 편차 : std_dev
# std_dev=math.sqrt(var)    
def get_std_dev(var):
    return round(math.sqrt(var),2)
    
    
def get_evaluation(avg, total_avg, std_dev, sd=20):
    if avg<total_avg and std_dev>20:
        print('성적이 너무 저조, 학생들의 실력차이가 너무 크다')
    elif avg>total_avg and std_dev>20:
        print('성적은 평균 이상이지만 학생들의 실력차이가 크다. 주의')
    elif avg<total_avg and std_dev<20:
        print('학생들의 실력차이는 크지 않지만 성적이 너무 저조, 주의')
    elif avg>total_avg and std_dev<20:
        print('성적 평균이상, 실력차이도 적음')
        
        
        
# test code
if __name__=='__main__':
    raw_data=get_raw_data('class_2-3.xlsx')
    print(raw_data) # {'greg': 95, 'john': 25, 'yang': 50, 'timothy': 15, 'melisa': 100, 'thor': 10, 'elen': 25, 'mark': 80, 'steve': 95, 'anna': 20}

    scores=get_scores(raw_data)
    print(scores) # (95, 25, 50, 15, 100, 10, 25, 80, 95, 20)

    avg=get_average(scores)
    print(avg) # 51.5

    var=get_variance(scores,avg)
    print(var) # 1240.25

    std_dev=get_std_dev(var)
    print(std_dev) #35.22

    get_evaluation(avg,60,std_dev) # 성적이 너무 저조, 학생들의 실력차이가 너무 크다
```

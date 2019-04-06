---
title: "procedure_oop"
date: 2019-04-06
categories: oop
---

- statistics.py 통계와 관련 있는 함수들만 따로 분리
```python
# utility class ( 관련 기능만 모아놓은 클래스 )
from functools import reduce
import math

# Single responsibility principle
# 통계에 대한 하나의 책임만 진다.
class Stat:
    # get_average(score) -> float(2)
    def get_average(self,score):
        """
        get_average(scores) -> float
        scores -> tuple 1f scores
        ret : float 소수점 이하 두자리 반환
        """
        avg=reduce(lambda a,b: a+b, score)/len(score)
        return round(avg,2)
        
# get_variance(...) -> float(2)
    def get_variance(self,scores,avg):
        """
        get_variance(scores,avg)
        scores -> tuple
        avg -> 평균
        ret : float 소수점 이하 두자리 반환
        """
        var=reduce(lambda res, x:(avg-x)**2+res, scores, 0)/len(scores)
        return round(var,2)
        
# get_std_dev() -> float(2)
    def get_std_dev(self,var):
        """
        get_std_dev(var) -> float
        var -> 분산
        ret : 표준편차, 소수점 이하 두자리 반환
        """
        return round(math.sqrt(var),2)
```


- datahandler.py 데이터 읽어오기, 연산, 평가, 결과 출력
```python
from statistics import *
import openpyxl

class DataHandler:
    # composition
    evaluator=Stat()
    
    @classmethod
    def get_raw_data(cls,filename):
        '''
        get_raw_data(filename) -> dict
        dict={'name1':score1,'name2':score2, ...}
        data : 학생=key, 점수=value -> dictionary
        '''
        raw_data={}
        wb=load_workbook(filename)
        ws=wb.active
        g=ws.rows

        for name_cell, score_cell in g:
            raw_data[name_cell.value]=score_cell.value
        return raw_data
        
    def __init__(self,filename):
        self.year_class=filename.split('_')[1]
        self.raw_data=DataHandler.get_raw_data(filename)
        self.scores=list(self.raw_data.values())
        
        # 연산한 값을 저장해 두는 저장소
        # 필요할 때 연산하되 이미 연산된 값이면 연산 없이 값을 반환
        self.cache={}
        
    def get_scores(self):
        '''
        만약 scores가 self.cache에 없다면
        self.cache에 scores를 넣고 self.raw_data의 값을 리스트 형식으로
        '''
        if 'scores' not in self.cache:
            self.cache['scores']=list(self.raw_data.values())
        return self.cache.get('scores')
        

    def get_average(self):
        if 'average' not in self.cache:
            self.cache['average']=self.calculator.get_average(self.get_scores())
        return self.cache.get('average')
        

    def get_variance(self):
        if 'variance' not in self.cache:
            self.cache['variance']=self.calculator.get_variance(self.get_scores(), self.get_average())
        return self.cache.get('variance')


    def get_std_dev(self):
        if 'std_dev' not in self.cache:
            self.cache['std_dev']=self.calculator.get_std_dev(self.get_variance())
        return self.cache.get('std_dev')


    def evaluate_class(self, total_avrg, sd=20):
        """
        evaluate_class(total_avrg, sd)->None
        total_avrg : 학년 전체 성적 평균
        sd : 원하는 표준편차 기준
        """
        avrg=self.get_average()
        std_dev=self.get_std_dev()

        if avrg < 50 and std_dev > sd:
            print('성적이 너무 저조하고 학생들의 실력 차이가 너무 크다.')
        elif avrg > 50 and std_dev > sd:
            print('성적은 평균 이상이지만 학생들의 실력 차이가 크다. 주의 요망!')
        elif avrg < 50 and std_dev < sd:
            print('학생들의 실력 차이는 크지 않지만 성적이 너무 저조하다. 주의 요망!')
        elif avrg > 50 and std_dev < sd:
            print('성적도 평균 이상이고 학생들의 실력 차이도 크지 않다.')

    def get_evaluation(self, total_avrg, sd=20):
        print('*'*50)
        print('{} 반 성적 분석 결과'.format(self.year_class))
        print('{} 반의 평균은 {}점이고 분산은 {}이며 표준편차는 {}이다.'.format(
            self.year_class,
            self.get_average(),
            self.get_variance(),
            self.get_std_dev()
        ))
        print('*'*50)
        print('{}반 종합 평가'.format(self.year_class))
        print('*'*50)
        self.evaluate_class(total_avrg, sd)
```

- main.py 유저가 실행하는 실행파일
```python
import sys
import datahandler

if not len(sys.argv)==3 and not len(sys.argv)==4:
    print('usage : python main.py <exel filename> <total_avg> <sd=20> 형태로 입력해주세요')
    exit(-1) # 여기서 강제 종료

dh=datahandler.DataHandler(sys.argv[1])

if len(sys.argv)==3:
    dh.get_evaluation(sys.argv[2])
elif len(sys.argv)==4:
    dh.get_evaluation(float(sys.argv[2]), float(sys.argv[3]))
```

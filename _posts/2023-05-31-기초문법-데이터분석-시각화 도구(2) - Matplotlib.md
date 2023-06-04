---
title: "[Python] 시각화 도구(2) - Matplotlib"
excerpt: "0531 시각화 도구(2) - Matplotlib"

categories:
  - 데이터 분석
tags:
  - []

permalink: /DA/lesson4/

toc: true
toc_sticky: ture

date: 2023-05-31
last_modified_at: 2023-06-04
---

- 한글 폰트 설치

```python
!sudo apt-get install -y fonts-nanum
!sudo fc-cache -fv
!rm ~/.cache/matplotlib -rf
```

- 데이터 불러오기

```python
import pandas as pd
df = pd.read_excel('/content/drive/MyDrive/hana1/PYTHON/data/시도별 전출입 인구수.xlsx', engine = 'openpyxl')

# NaN값 채우기, ffill: 바로 위 값과 동일하게 설정
df = df.fillna(method = 'ffill')

# 서울시(전출지)에서 다른 지역(전입지)으로 이동한 인구 데이터
b_index = (df['전출지별'] == '서울특별시') & (df['전입지별'] != '서울특별시')
df_seoul = df[b_index]

# 전출지별 열 삭제
df_seoul = df_seoul.drop('전출지별', axis = 1)

# 열 이름 변경
df_seoul = df_seoul.rename({'전입지별':'전입지'}, axis = 1)

# 행 인덱스 변경
df_seoul = df_seoul.set_index('전입지')

# 전입지가 경기도인 경우만 추출
df_ggd = df_seoul.loc['경기도']
df_ggd.head()
--------------------------------------------------------
# 1970    130149
# 1971    150313
# 1972     93333
# 1973    143234
# 1974    149045
# Name: 경기도, dtype: object
```

## 그래프 생성 (선 그래프)

```python
import matplotlib.pyplot as plt
plt.plot(df_ggd.index, df_ggd.values)
```

```python
# 시리즈 데이터를 넣고 시각화해도 결과는 동일
plt.plot(df_ggd)
```
![image](https://github.com/JS042/cs231n/assets/84077022/33e698ed-4204-4974-a43e-0f426ee5b751)


## 차트 제목, 축 제목 추가

```python
# 한글 폰트 지정
plt.rc('font', family = 'NanumGothic')

# 선 그래프
plt.plot(df_ggd.index, df_ggd.values)

# 차트 제목 추가
plt.title("서울시에서 경기도로 이동하는 인구")

# 축 제목 추가
plt.xlabel('기간(연도)')
plt.ylabel('이동 인구 수')
plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/d8784cb1-ca4b-4e0f-9ce5-57f81d099aed)

## 그래프 꾸미기

```python
# 그림 사이즈 지정(가로, 세로)
plt.figure(figsize = (13,5))

# x축 눈금 라벨 회전
plt.xticks(rotation = 'vertical')

# 선 그래프
plt.plot(df_ggd.index, df_ggd.values)

# 차트 제목 추가
plt.title("서울시에서 경기도로 이동하는 인구")

# 축 제목 추가
plt.xlabel('기간(연도)')
plt.ylabel('이동 인구 수')

# 범례 추가
plt.legend(labels = ['서울시 => 경기도'], loc = 'best')
plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/bd5515be-ce73-43ae-81e2-5eef5e2c8196)

## 스타일 서식

```python
plt.style.available
-----------------------
# ['Solarize_Light2',
#  '_classic_test_patch',
#  '_mpl-gallery',
#  '_mpl-gallery-nogrid',
#  'bmh',
#  'classic',
#  'dark_background',
#  'fast',
#  'fivethirtyeight',
#  'ggplot',
#  'grayscale',
#  'seaborn-v0_8',
#  'seaborn-v0_8-bright',
#  'seaborn-v0_8-colorblind',
#  'seaborn-v0_8-dark',
#  'seaborn-v0_8-dark-palette',
#  'seaborn-v0_8-darkgrid',
#  'seaborn-v0_8-deep',
#  'seaborn-v0_8-muted',
#  'seaborn-v0_8-notebook',
#  'seaborn-v0_8-paper',
#  'seaborn-v0_8-pastel',
#  'seaborn-v0_8-poster',
#  'seaborn-v0_8-talk',
#  'seaborn-v0_8-ticks',
#  'seaborn-v0_8-white',
#  'seaborn-v0_8-whitegrid',
#  'tableau-colorblind10']
```

```python
# 스타일 서식
plt.style.use('ggplot')

# 그림 사이즈 지정(가로, 세로)
plt.figure(figsize = (13,5))

# x축 눈금 라벨 회전
plt.xticks(rotation = 'vertical')

# 선 그래프
plt.plot(df_ggd.index, df_ggd.values)

# 차트 제목 추가
plt.title("서울시에서 경기도로 이동하는 인구")

# 축 제목 추가
plt.xlabel('기간(연도)')
plt.ylabel('이동 인구 수')

# 범례 추가
plt.legend(labels = ['서울시 => 경기도'], loc = 'best')
plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/b9ad7304-c106-4235-8f21-a6eb2ae1c3c4)

## 주석 추가

```python
# 스타일 서식
plt.style.use('ggplot')

# 그림 사이즈 지정(가로, 세로)
plt.figure(figsize = (13,5))

# x축 눈금 라벨 회전
plt.xticks(rotation = 'vertical')

# 선 그래프
plt.plot(df_ggd.index, df_ggd.values)

# 차트 제목 추가
plt.title("서울시에서 경기도로 이동하는 인구")

# 축 제목 추가
plt.xlabel('기간(연도)')
plt.ylabel('이동 인구 수')

# 범례 추가
plt.legend(labels = ['서울시 => 경기도'], loc = 'best', fontsize = 15)

# y축 범위 조정
plt.ylim(50000,800000)

# 주석 추가
# 화살표
plt.annotate('', xy = (23,620000), # 화살표 머리
             xytext = (3,250000), # 화살표 꼬리
             xycoords = 'data', # 좌표계
             arrowprops = dict(arrowstyle = '->', color = 'red', lw = 10))
plt.annotate('', xy = (45,400000), # 화살표 머리
            xytext = (29,620000), # 화살표 꼬리
             xycoords = 'data', # 좌표계
             arrowprops = dict(arrowstyle = '->', color = 'blue', lw = 10))

# 텍스트 추가
plt.annotate('인구 이동 증가',
             xy = (12, 400000), # 텍스트 시작 위치
             rotation = 25, # 회전
             va = 'baseline', # 위아래 정렬
             ha = 'center', # 좌우로 정
             fontsize = 15)
plt.annotate('인구 이동 감소',
             xy = (37, 510000), # 텍스트 시작 위치
             rotation = -18, # 회전
             va = 'baseline', # 위아래 정렬
             ha = 'center', # 좌우로 정
             fontsize = 15)

plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/8aca4f11-420e-4aaa-a88b-7539e141d815)

## 화면 분할

```python
df_4 = df_seoul.loc[['충청남도','경상북도','강원도','전라남도']]
col_years = list(map(str, range(1970,2018)))

# 그래프 객체 만들기
fig = plt.figure(figsize = (20,10))
ax1 = fig.add_subplot(2,2,1) # 행 개수, 열 개수, 위치
ax2 = fig.add_subplot(2,2,2)
ax3 = fig.add_subplot(2,2,3)
ax4 = fig.add_subplot(2,2,4)

# axe 객체에 그래프 추가
ax1.plot(col_years, df_4.loc['충청남도',:], marker = 'o', markerfacecolor = 'green',
         markersize = 5, color = 'green', label = '서울 => 충남')
ax2.plot(col_years, df_4.loc['경상북도',:], marker = 'o', markerfacecolor = 'red',
         markersize = 5, color = 'red', label = '서울 => 경북')
ax3.plot(col_years, df_4.loc['강원도',:], marker = 'o', markerfacecolor = 'yellow',
         markersize = 5, color = 'yellow', label = '서울 => 강원')
ax4.plot(col_years, df_4.loc['전라남도',:], marker = 'o', markerfacecolor = 'blue',
         markersize = 5, color = 'blue', label = '서울 => 전남')

# 범례 추가
ax1.legend(loc = 'best')
ax2.legend(loc = 'best')
ax3.legend(loc = 'best')
ax4.legend(loc = 'best')

# x축 눈금 라벨 회전
ax1.set_xticklabels(col_years, rotation = 90)
ax2.set_xticklabels(col_years, rotation = 90)
ax3.set_xticklabels(col_years, rotation = 90)
ax4.set_xticklabels(col_years, rotation = 90)

# y축 범위 조정
ax1.set_ylim(0,60000)
ax2.set_ylim(0,60000)
ax3.set_ylim(0,60000)
ax4.set_ylim(0,60000)

# 스타일 서식
plt.style.use('ggplot')

plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/760258cd-4142-41d8-997c-e57350265fdb)

```python
# 한글 폰트 지정
plt.rc('font', family = 'NanumGothic')

# 스타일 서식
plt.style.use('ggplot')

# 그래프 객체 생성
fig = plt.figure(figsize = (10,10))
ax1 = fig.add_subplot(2,1,1) # 행의 개수, 열의 개수, 위치
ax2 = fig.add_subplot(2,1,2)

# 객체에 그래프 출력
ax1.plot(df_ggd, marker = 'o', markersize = 10)
ax2.plot(df_ggd, marker = 'o', markersize = 10,
         markerfacecolor = 'green', color = 'lightgreen', linewidth = 5, label = '서울 => 경기')
# ax2 객체에 범례 추가
ax2.legend(loc = 'best')

# y축 범위 조정
ax1.set_ylim(0,700000)
ax2.set_ylim(0,700000)

# x축 눈금 라벨 회전
ax1.set_xticklabels(df_ggd.index, rotation = 75)
ax2.set_xticklabels(df_ggd.index, rotation = 75)

# 차트 제목 추가
ax2.set_title('서울시에서 경기도로 이동한 인구', size = 15)

# 축 이름 추가
ax2.set_xlabel('기간(연도)', size = 10)
ax2.set_ylabel('이동 인구 수', size = 10)

# 축 눈금 라벨 크기 수정
ax1.tick_params(axis = 'both', labelsize = 8)

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/57a657cb-ed1e-4b8a-9367-a5dd8d20f3e8)

## 색상 & 헥스 코드

```python
import matplotlib
colors = {}

for i,j in matplotlib.colors.cnames.items():
  colors[i] = j

print(colors)
-------------------------------------------
# {'aliceblue': '#F0F8FF',
#  'antiquewhite': '#FAEBD7',
#  'aqua': '#00FFFF',
#  'aquamarine': '#7FFFD4',
#  'azure': '#F0FFFF',
#  'beige': '#F5F5DC',
#  'bisque': '#FFE4C4',
#  'black': '#000000',
# ...}
```

## 면적 그래프

- 일반 면적 그래프

```python
df_4 = df_seoul.loc[['충청남도','경상북도','강원도','전라남도']]
df_4_t = df_t.T

# kind 매개변수 기본값은 선
df_4_t.plot(kind = 'area', 
            stacked = False, # 누적 여부
            alpha = 0.2, # 투명도
            figsize = (7,5))

# 차트 제목
plt.title('서울시에서 다른 4개 지역으로 이동한 인구')

# 축 제목
plt.xlabel('기간(연도)', size = 10)
plt.ylabel('이동 인구 수', size = 10)

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/8f4f73ad-3cce-4e2b-968b-3eb55749758e)

- 누적 면적 그래프

```python
# 누적 면적 그래프
# kind 매개변수 기본값은 선
df_4_t.plot(kind = 'area', 
            stacked = True, # 누적 여부
            alpha = 0.2, # 투명도
            figsize = (7,5))

# 차트 제목
plt.title('서울시에서 다른 4개 지역으로 이동한 인구')

# 축 제목
plt.xlabel('기간(연도)', size = 10)
plt.ylabel('이동 인구 수', size = 10)

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/ee2ee732-693c-4a08-b690-0f5115a547cd)


## 막대 그래프

- 수직 막대 그래프

```python
# 2000년 이후 데이터 추출
df_4_new = df_4.loc[:,'2000':].T

# 수직 막대 그래프
df_4_new.plot(kind = 'bar',
          figsize =  (12,5),
          width = 0.5, # 막대 굵기
          color = ['green','red','yellow','blue'])

# 차트 제목
plt.title('서울시에서 다른 4개 지역으로 이동한 인구', size = 20,
               color = 'red', weight = 'bold')

# 축 제목
plt.xlabel('기간(연도)', size = 10, color = 'blue')
plt.ylabel('이동 인구 수', size = 10, color = 'blue')

plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/0f0ddf98-f377-4f20-9752-990394bfcf3b)

- 수직 막대 그래프

```python
# 수직 막대 그래프
df_4_new.plot(kind = 'barh',
          figsize =  (12,5),
          width = 0.5, # 막대 굵기
          color = ['green','red','yellow','blue'])

# 차트 제목
plt.title('서울시에서 다른 4개 지역으로 이동한 인구', size = 20,
               color = 'red', weight = 'bold')

# 축 제목
plt.xlabel('기간(연도)', size = 10, color = 'blue')
plt.ylabel('이동 인구 수', size = 10, color = 'blue')

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/34336af3-db0f-4b68-928f-6c439a6bde3f)


- 수직 막대 vs 수평 막대

```python
# 인구의 총합 데이터
df_total = df_4_new.T
df_total['합계'] = df_4_new.sum()
df_total_dsc = df_total[['합계']].sort_values(by = '합계', ascending = False)
print(df_total_dsc)
---------------------------------------------------------
#           합계
# 전입지         
# 충청남도  434258
# 강원도   398481
# 전라남도  302765
# 경상북도  265010
```

```python
df_total_dsc = df_total[['합계']].sort_values(by = '합계', ascending = False)
df_total_asc = df_total[['합계']].sort_values(by = '합계')

# 한글 폰트 지정
plt.rc('font', family = 'NanumGothic')

# 스타일 서식
plt.style.use('ggplot')

# 그래프 객체 생성
fig  = plt.figure(figsize = (10,5))
ax1 = fig.add_subplot(1,2,1)
ax2 = fig.add_subplot(1,2,2)

# 수직 막대 그래프
df_total_dsc.plot(kind = 'bar', 
                  figsize = (12,5),
                  width = 0.5,
                  color = 'blue',
                  ax = ax1)
# 수평 막대 그래프 - 축 전환으로 인해 정렬 순서가 역순
df_total_asc.plot(kind = 'barh', 
                  figsize = (12,5),
                  width = 0.5,
                  ax = ax2)

# 차트 제목
ax1.set_title('서울시에서 다른 4개 지역으로 이동한 인구')
ax2.set_title('서울시에서 다른 4개 지역으로 이동한 인구')

# 축 제목
ax1.set_xlabel('이동 인구 수', size = 10)
ax2.set_ylabel('전입지', size = 10)

# x축 조정
ax1.set_xticklabels(df_total_dsc.index, rotation = 0)

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/ac58f369-0a64-45e9-8a37-7cb111299c79)

## 보조축 - 2축 그래프

```python
# 데이터 불러오기
df = pd.read_excel('/content/drive/MyDrive/hana1/PYTHON/data/남북한발전전력량.xlsx', 
                   engine = 'openpyxl')

# 북한 데이터 추출
df_north = df.iloc[5:]
df_north = df_north.drop('전력량 (억㎾h)', axis = 1)
df_north = df_north.set_index('발전 전력별')
df_north = df_north.T

# 전력 증감율 계산
df_north = df_north.rename(columns = {'합계': '총발전량'})
df_north['전년총발전량'] = df_north['총발전량'].shift(1)

# 증감율 = ((금년총발전량 - 전년총발전량) / 전년총발전량) * 100
# 총발전량 = 금년총발전량
df_north['증감율'] = ((df_north['총발전량'] - df_north['전년총발전량']) / df_north['전년총발전량']) * 100
df_north.head()
----------------------------------------------------
# 발전 전력별 총발전량   수력   화력 원자력 전년총발전량        증감율
# 1990    277  156  121   -   None        NaN
# 1991    263  150  113   -    277  -5.054152
# 1992    247  142  105   -    263   -6.08365
# 1993    221  133   88   -    247 -10.526316
# 1994    231  138   93   -    221   4.524887
```

```python
plt.rc('font', family = 'NanumGothic')
plt.style.use('ggplot')

# 마이너스 기호 출력
plt.rcParams['axes.unicode_minus'] =  False

# axe 객체
# 수력, 화력 발전에 대한 누적 막대 그래프
ax1 = df_north[['수력','화력']].plot(kind = 'bar', stacked = True, figsize = (12,6))
ax2 = ax1.twinx() # x축 공유

# 증감율에 대한 선 그래프
ax2.plot(df_north.index, df_north['증감율'], color = 'green', marker = 'o', label = '증감율')

# y축 범위 조정
ax1.set_ylim(0,500)
ax2.set_ylim(-60,40)

# 차트 제목
plt.title('북한 전력 발전량')

# 축 제목
ax1.set_xlabel('기간(연도)')
ax1.set_ylabel('발전량')
ax2.set_ylabel('증감율')

# 범례
ax1.legend(loc = 'upper left')
ax2.legend(loc = 'best')

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/d60d8e9f-fc7b-400e-b275-c202c4743dfa)

## 히스토그램

```python
# 데이터 불러오기
# csv 파일 불러오기 + 열 이름이 없음(header = None)
df = pd.read_csv('/content/drive/MyDrive/hana1/PYTHON/data/auto-mpg.csv', header = None)

# 열 이름 지정
df.columns = ['mpg', 'cylinders', 'displacement', 'horsepower', 'weight', 
              'acceleration', 'model_year', 'origin', 'car_name']
```

```python
# 한글 폰트 설정
plt.rc("font", family = "NanumGothic")
# 스타일 서식
plt.style.use("ggplot")

# 히스토그램
df['mpg'].plot(kind = 'hist', 
               bins = 15, # 구간 개수
               color = 'aquamarine',
               figsize = (5,3))

# 차트 제목
plt.title('자동차 연비 히스토그램')

# 축 제목
plt.xlabel('연비')

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/47e52c3c-1375-4fd5-ad7c-ded85c730dad)


## 산점도

- 일반 산점도

```python
# 스타일 서식
plt.style.use("ggplot")

# 산점도
df.plot(kind = 'scatter',
        x = 'mpg',
        y = 'weight',
        c = 'tomato', # 색상
        s = 20, # 점의 크기
        figsize = (7,5))

# 차트 제목
plt.title('차중과 연비의 산점도')

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/19b4b911-760e-4d51-b799-b65d5abf197a)

- 버블 차트 = 원의 크기로 그리기

```python
# 스타일 서식
plt.style.use("ggplot")

# cylinders 전처리
cy = df.cylinders * 100

# 산점도
df.plot(kind = 'scatter',
        x = 'mpg',
        y = 'weight',
        c = 'tomato', # 색상
        s = 'displacement', # 점의 크기
        alpha = 0.2,
        figsize = (7,5))

# 차트 제목
plt.title('차중과 연비, 배기량의 버블차트')

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/150fc793-d2c3-435b-ba84-a90e8d259942)

```python
# 스타일 서식
plt.style.use("ggplot")

# cylinders 전처리
cy = df.cylinders * 100

# 산점도
df.plot(kind = 'scatter',
        x = 'mpg',
        y = 'weight',
        c = 'tomato', # 색상
        s = cy, # 크기
        alpha = 0.2,
        figsize = (8,5))

# 차트 제목
plt.title("차중, 연비, 실린더의 버블 차트")

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/b01fb35e-9003-45e2-8765-8e332c96dacb)

- 원의 색상으로 그리기  - 숫자형 변수

```python
# 스타일 서식
plt.style.use("ggplot")

# 산점도
df.plot(kind = 'scatter',
        x = 'mpg',
        y = 'weight',
        c = 'displacement', # 색상
        cmap = 'plasma', # 컬러맵
        s = 10, # 크기
        alpha = 0.2,
        figsize = (8,5))

# 차트 제목
plt.title("차중, 연비, 배기량의 버블 차트")

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/844e74e0-a554-42d9-b1cb-a926ef2c4388)

- 원의 색상으로 그리기 - 범주형 변수    

```python
# 스타일 서식
plt.style.use("ggplot")

# origin 전처리
origin_cat = df.origin.astype('category')

# 산점도
df.plot(kind = 'scatter',
        x = 'mpg',
        y = 'weight',
        c = origin_cat, # 색상
        cmap = 'plasma', # 컬러맵
        s = 10, # 크기
        alpha = 0.2,
        figsize = (8,5))

# 차트 제목
plt.title("차중, 연비, 배기량의 버블 차트")

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/13569fac-6298-47ff-a8c5-fe12f132d226)

## 파이 차트

```python
df['count'] = 1
df_origin = df.groupby('origin').sum()
df_origin.index = ['USA','EU','JAP']
print(df_origin)
----------------------------------------------
# 	mpg	cylinders	displacement	weight	acceleration	model_year	count
# USA	5000.8	1556	61229.5	837121.0	3743.4	18827	249
# EU	1952.4	291	7640.0	169631.0	1175.1	5307	70
# JAP	2405.6	324	8114.0	175477.0	1277.6	6118	79
```

```python
# 스타일 서식
plt.style.use('ggplot')

# 파이 차트
df_origin['count'].plot(kind = 'pie',
                        figsize = (10,5),
                        autopct = '%1.1f%%', # 소수점 첫째자리로 %와 함께 표시
                        colors = ['red','blue','green'], # 각 조각의 색상
                        startangle = 0) # 시작 위치

# 차트 제목
plt.title('origin에 대한 원 그래프')

# 범례
plt.legend(loc = 'best')

plt.show()
```
![image](https://github.com/JS042/cs231n/assets/84077022/7e207dce-24db-45bb-8ad3-cf426aa64936)

## 상자 수염 그림
```python
# origin별로 연비 데이터 만들기
mpg_1 = df[df['origin'] == 1]['mpg']
mpg_2 = df[df['origin'] == 2]['mpg']
mpg_3 = df[df['origin'] == 3]['mpg']
```

```python
# 그림 객체
fig = plt.figure(figsize = (10,5))
ax = fig.add_subplot(1,1,1)
ax.boxplot(x = [mpg_1,mpg_2,mpg_3],
           labels = ['USA','EU','JAP'])

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/72685a64-bcb1-4dc0-9d5d-5ab8c3a674d8)

```python
# 그림 객체
fig = plt.figure(figsize = (10,5))
ax = fig.add_subplot(1,1,1)
ax.boxplot(x = [mpg_1,mpg_2,mpg_3],
           labels = ['USA','EU','JAP'],
           vert = False)

plt.show()
```

![image](https://github.com/JS042/cs231n/assets/84077022/869413ad-76aa-45cc-bbc1-8d641fe0008d)

## 참고자료

- 파이썬 그래프 갤러리  
https://www.python-graph-gallery.com/

- 컬러맵  

```python
plt.colormaps()
--------------
# ['magma',
#  'inferno',
#  'plasma',
#  'viridis',
#  'cividis',
#  'twilight',
#  'twilight_shifted',
# ...]
```
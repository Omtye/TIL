
## Pandas란?

데이터 구조 및 데이터 분석 도구를 제공하는 Python 라이브러리입니다. 엑셀과 상당히 유사하며 excel, csv, sql 등 다양한 데이터를 가져와서 처리할 수 있습니다. 

<br>

### Pandas의 장점

- 데이터 가독성이 높다
- 대용량 데이터를 효율적으로 처리할 수 있다
- 데이터 분석에 대한 다양한 기능을 제공
<br>

### Pandas 패키지 임포트

판다스 패키지를 사용하기 위해서는 우선 import를 해야하며 일반적으로 pd 라는 별칭으로 사용합니다.

```python
import pandas as pd
```
<br>

## 데이터프레임

데이터 프레임은 Pandas에서 제공하는 데이터 구조로 2차원의 행렬 데이터이다. 일반적인 데이터베이스의 구조라고 이해하면 됩니다.

<br>

### 데이터 프레임 생성

1. 하나의 열이 되는 데이터를 리스트나 일차원 배열을 준비합니다.
2. 각각의 열에 대한 이름을 키로 가지는 딕셔너리 생성합니다.
3. 이 데이터를 `DataFrame` 클래스 생성자에 넣습니다.
    - 열방향 인덱스는 `columns` 인수로, 행방향 인덱스는 `index` 인수로 지정

```python
data = {
    "2015": [9904312, 3448737, 2890451, 2466052],
    "2010": [9631482, 3393191, 2632035, 2431774],
    "2005": [9762546, 3512547, 2517680, 2456016],
    "2000": [9853972, 3655437, 2466338, 2473990],
    "지역": ["수도권", "경상권", "수도권", "경상권"],
    "2010-2015 증가율": [0.0283, 0.0163, 0.0982, 0.0141]
}
columns = ["지역", "2015", "2010", "2005", "2000", "2010-2015 증가율"]
index = ["서울", "부산", "인천", "대구"]
df = pd.DataFrame(data, index=index, columns=columns)
df
```

![Untitled](https://user-images.githubusercontent.com/43038052/123983129-96366580-d9fe-11eb-852c-c4151363e542.png)

데이터에 접근하려면 `values` 속성을 사용합니다. 열, 행 방향 인덱스는 각각 `columns` , `index` 사용

```python
##데이터 표시
df.values

##열 표시
df.columns

##행 표시
df.index
```
<br>

### 열 데이터의 갱신, 추가, 삭제

'2010-2015 증가율' 열 값을 백분율로 변경

```python
df['2010-2015 증가율'] = df['2010-2015 증가율'] * 100
df
```

![Untitled 1](https://user-images.githubusercontent.com/43038052/123981998-ae59b500-d9fd-11eb-98db-e8889197deab.png)

'2005-2010 증가율' 이라는 이름의 열 추가

```python
df['2005-2010 증가율'] = ((df['2010'] - df['2005']) / df['2005'] * 100).round(2)
df
```

![Untitled 2](https://user-images.githubusercontent.com/43038052/123982000-ae59b500-d9fd-11eb-8737-392d3b972ff3.png)

<br>

### "2010-2015 증가율"이라는 이름의 열 삭제

```python
del df['2010-2015 증가율']
df
```

![Untitled 3](https://user-images.githubusercontent.com/43038052/123982002-aef24b80-d9fd-11eb-9583-8138d6c926e8.png)

<br>

### 열 인덱싱

데이터 프레임을 인덱싱 할 때 열 라벨을 키값으로 인덱싱을 할 수 있습니다. 인덱스로 라벨 값을 하나만 넣으면 시리즈 객체가 반환되고 라벨의 배열 또는 리스트를 넣으면 부분적인 데이터프레임이 반환됩니다.

```python
## 하나의 열만 인덱싱하면 시리즈가 반환된다.
df['지역']
```

![Untitled 4](https://user-images.githubusercontent.com/43038052/123982007-aef24b80-d9fd-11eb-8b04-a4f27cfc98ac.png)

데이터 프레임 자료형을 유지하고 싶다면 열에 대괄호 추가

```python
# 2010이라는 열을 반환하면서 데이터프레임 자료형을 유지
df[['2010']]
```

![Untitled 5](https://user-images.githubusercontent.com/43038052/123982011-af8ae200-d9fd-11eb-8df0-e462c6614cf9.png)

```python
# 2010이라는 열을 반환하면서 시리즈 자료형으로 변환
df['2010']
```

![Untitled 6](https://user-images.githubusercontent.com/43038052/123982013-af8ae200-d9fd-11eb-87bb-5eb2be606d4b.png)

<br>

### 행 인덱싱

행 단위로 인덱싱을 하기 위해서는 슬라이싱을 해야하며 인덱스 값이 문자열이면 라벨 슬라이싱도 가능합니다. (인덱스는 0부터 시작합니다.)

```python
## 0번째 행 
df[:1]

## 0~1 번째 행
df[1:2]

## 0~2
df[1:3]

## 라벨 슬라이싱
df['서울':'부산']
```

![Untitled 7](https://user-images.githubusercontent.com/43038052/123982015-b0237880-d9fd-11eb-8b72-50642fe887c9.png)

<br>

## 데이터 입출력

Pandas는 데이터 파일을 읽어 데이터프레임을 만들 수 있는데 다양한 포맷을 지원합니다.

- CSV
- Excel
- HTML
- JSON
- HDF5
- SAS
- STATA
- SQL

<br>

### CSV 파일 입력

csv 파일로부터 데이터를 읽어 데이터프레임을 만들 때는 `pandas.read_csv` 함수를 사용합니다. 만약에 csv 파일에 열 인덱스 정보가 없는 경우에는 names 인수로 설정이 가능합니다.

혹시나 csv 아닌 파일에서 구분자가 콤마가 아니라면 `sep` 인수를 사용하여 구분자를 사용자가 지정해줍니다. 공백의 경우 `\s+` 정규식 문자열

```python
pd.read_csv('data/sample.csv')

## 열 인덱스가 없을경우
pd.read_csv('data/sample1.csv', names = ['c1','c2','c3'])

## 특정 열 값을 인덱스로 지정하고 싶은 경우
pd.read_csv('data/sample1.csv', index_col='c3')

## csv 파일이 아닌 경우(공백이 구분자)
pd.read_table('data/sample1.csv', sep='\s+')

## 건너뛰어야할 행이 있을 경우 (첫번째 행 제거)
pd.read_csv('data/sample1.csv', skiprows=[0, 0])
```

특정값을 NaN으로 표시하고 싶은 경우 `na_values` 인수에 NaN 값으로 취급할 값을 넣습니다.

```python
df = pd.read_csv('sample5.csv', na_values=['누락'])
df
```

![Untitled 8](https://user-images.githubusercontent.com/43038052/123982019-b0bc0f00-d9fd-11eb-9750-96aced659b66.png)

<br>

### CSV 파일 출력

데이터프레임을 값을 CSV 파일로 출력하고 싶은 경우에는 `to_csv` 메소드를 사용합니다.

```python
df.to_csv('sample6.csv')
```

<br>

## 데이터프레임 고급 인덱싱

데이터프레임에서 특정한 데이터만 골라내는 것을 인덱싱이라고 합니다. Pandas는 numpy 행렬과 같이 쉼표를 사용한 (행 인덱스, 열 인덱스) 형식의 2차원 인덱싱을 지원합니다.

- loc : 라벨값 기반의 2차원 인덱싱
- iloc : 순서를 나타내는 정수 기반의 2차원 인덱싱

<br>

### loc 인덱서

`loc` 인덱서는 아래 처럼 사용합니다.

```python
df.loc[행 인덱싱값]
##또는
df.loc[행 인덱싱값, 열 인덱싱값]
```

```python
## sample DataFram
df = pd.DataFrame(np.arange(10, 22).reshape(3, 4),
                  index=["a", "b", "c"],
                  columns=["A", "B", "C", "D"])
```

인덱스가 'a' 인 행을 선택하면 해당 행이 시리즈로 출력됩니다.

```python

df.loc['a']
```

![Untitled 9](https://user-images.githubusercontent.com/43038052/123982021-b0bc0f00-d9fd-11eb-9fe7-a969b2bbcb98.png)

인덱스를 지정해서 범위 슬라이스도 가능합니다.

```python
df.loc['b':'c']

#동일한 결과값
df['b':'c']
```

![Untitled 10](https://user-images.githubusercontent.com/43038052/123982025-b0bc0f00-d9fd-11eb-8601-001f91ebfe28.png)

특정 인덱스를 지정하여 추출할 수 있습니다.

```python
## a,c 인덱스 값
df.loc[['a', 'c']]
```

![Untitled 11](https://user-images.githubusercontent.com/43038052/123982026-b154a580-d9fd-11eb-8ee2-34fbccc56564.png)
조건식을 지정해서 결과값에 만족하는 행을 추출할 수 있습니다.

또는 인덱스 값을 반환하는 함수를 지정하여 사용하는 것도 가능합니다.

```python
## df.A 값이 15보다 큰 인덱스 값
df.loc[df.A > 15]
```

```python
## 함수를 선언하여 인덱스 값을 추출
def select_rows(df):
    return df.A > 15

df.loc[select_rows(df)]
```

![Untitled 12](https://user-images.githubusercontent.com/43038052/123982030-b154a580-d9fd-11eb-870b-0c57cb45f668.png)

추가로 인덱스로 지정되지 않은 값을 지정할 경우 오류가 발생하게 된다.

```python
## 오류 발생 A 는 열값
df.loc['A'] 

## A값을 추출하고 싶은 경우
df['A']
```

다만 행과 열을 같이 지정하여 사용할 수는 있습니다. `df.loc[행 인덱스, 열 인덱스]` 형태로 사용이 가능합니다. 

이때 반드시 행, 열 순서로 지정해야합니다

```python
## 10 출력
df.loc['a','A']
```

라벨 데이터 슬라이싱을 사용할 수 있습니다.

```python
df.loc["b":, "A"]
```

![Untitled 13](https://user-images.githubusercontent.com/43038052/123982033-b1ed3c00-d9fd-11eb-9e77-7ca001ff36e5.png)

```python
df.loc["a", :]
```

![Untitled 14](https://user-images.githubusercontent.com/43038052/123982035-b1ed3c00-d9fd-11eb-97f8-bb67aa1f8478.png)

```python
# C,D열에서 A가 10보다 큰 것
df.loc[df.A > 10, ["C", "D"]]
```

![Untitled 15](https://user-images.githubusercontent.com/43038052/123982038-b285d280-d9fd-11eb-96e8-fd61ad59c433.png)

<br>

### iloc 인덱서

iloc 인덱서는 라벨 name이 아닌 `location`을 지정하는 방법으로 인덱스는 정수 형태이며 사용법은 loc 와 동일합니다.

```python
# 11 출력
df.iloc[0,1]
```

```python
df.iloc[:2, 2]
```

![Untitled 16](https://user-images.githubusercontent.com/43038052/123982042-b285d280-d9fd-11eb-9aa0-f0d16ac2dc2d.png)

여기서 `-` 값은 시작점에서 끝점으로 가는 방향으로 이해하면 됩니다.

```python
df.iloc[0, -2:]
```

![Untitled 17](https://user-images.githubusercontent.com/43038052/123982045-b31e6900-d9fd-11eb-9b7c-2afba0bc4ac3.png)

```python
df.iloc[2:3,1:3]
```

![Untitled 18](https://user-images.githubusercontent.com/43038052/123982046-b31e6900-d9fd-11eb-9af5-45bb5f9b7e83.png)

<br>

## 데이터프레임 데이터 조작

<br>

### 데이터 갯수 세기

데이터 갯수를 셀 경우 `count` 를 사용합니다. 이 때 NaN 값은 제외합니다.

```python

s = pd.Series(range(10))
s[3] = np.nan
s
```

![Untitled 19](https://user-images.githubusercontent.com/43038052/123982048-b3b6ff80-d9fd-11eb-8f81-3134447f2458.png)

```python
s.count()
```

![Untitled 20](https://user-images.githubusercontent.com/43038052/123982050-b3b6ff80-d9fd-11eb-875b-298481e777bc.png)
데이터 프레임의 경우 각 열마다 데이터 갯수를 센다.

```python
np.random.seed(2)
df = pd.DataFrame(np.random.randint(5, size=(4,4)), dtype=float)
df.iloc[2,3] = np.nan
df
```

![Untitled 21](https://user-images.githubusercontent.com/43038052/123982052-b44f9600-d9fd-11eb-9d82-4bd96138eb76.png)

```python
df.count()
```

![Untitled 22](https://user-images.githubusercontent.com/43038052/123982055-b44f9600-d9fd-11eb-8735-c4f0e95bc145.png)

<br>

### 타이타닉 연습문제 1

`seaborn` 패키지를 import 하면 아래와 같이 타이타닉 승객 데이터를 데이터프레임으로 읽어 올 수 있다.

```python
import seaborn as sns
titanic = sns.load_dataset('titanic')
titanic.head()
```

![Untitled 23](https://user-images.githubusercontent.com/43038052/123982057-b44f9600-d9fd-11eb-9f0f-63b4b8ce1efe.png)

```python
# 타이타닉호 열마다 데이터 개수
titanic.count()
```

![Untitled 24](https://user-images.githubusercontent.com/43038052/123982059-b4e82c80-d9fd-11eb-8587-e5edca09445b.png)

<br>

### 카테고리 값 세기

시리즈 값이 정수, 문자열, 카테고리 값인 경우에는 `value_counts` 메소드로 각각의 값이 나온 횟수를 셀 수 있다.

```python
np.random.seed(1)
s2 = pd.Series(np.random.randint(6, size=100))
s2.tail()
```

![Untitled 25](https://user-images.githubusercontent.com/43038052/123982063-b4e82c80-d9fd-11eb-92aa-660b34a0b965.png)

```python
s2.value_counts()
```

![Untitled 26](https://user-images.githubusercontent.com/43038052/123982066-b580c300-d9fd-11eb-9651-c570549c2120.png)

단, 데이터프레임의 경우 각 열마다 `value_counts` 메소드를 실행해야만 합니다.

이때 `normalize=True` 인수를 적용할 경우 비율을 확인 할 수 있습니다.

```python
df[0].value_counts()

df[0].value_counts(normalize=True)
```
<br>

### 정렬

아까 위에서 나온 결과값이 1,0,4,5,3,2 로 상당히 거슬렸는데 데이터를 정렬하려면

`sort_index` , `sort_values` 메소드를 사용합니다.

`sort_index` 는 인덱스 값을 기준으로 `sort_values` 는 데이터 값을 기준으로 정렬합니다. 

- 내림 차순으로 정렬할 경우에는 `ascending=False` 인수를 지정해줍니다.

```python
s2.value_counts().sort_index()

# 내림차순의 경우
s2.value_counts().sort_index(ascending=False)
```

![Untitled 27](https://user-images.githubusercontent.com/43038052/123982067-b580c300-d9fd-11eb-8ac3-aa3e14bffb90.png)
만약 정렬 시 NaN 값이 있는 경우 가장 마지막으로 정렬됩니다.

```python
s.sort_values()

# 내림차순의 경우
s.sort_values(ascending=False)
```

![Untitled 28](https://user-images.githubusercontent.com/43038052/123982069-b6195980-d9fd-11eb-861a-146cec1b15c0.png)

데이터 프레임에서 정렬 메소드 사용시 `by` 인스로 정렬 기준이 되는 열을 지정해주어야 합니다.

```python
df.sort_values(by=1)
```

![Untitled 29](https://user-images.githubusercontent.com/43038052/123982072-b6195980-d9fd-11eb-9fe2-ab07bcad82ba.png)

<br>

### 타이타닉 연습문제 2

타이타닉호 승객에 대해 성별(sex) 인원수, 나이별(age) 인원수, 선실별(class) 인원수, 사망/생존(alive) 인원수를 구해보자

```python
# 성별 인원수
titanic['sex'].value_counts()
```

![Untitled 30](https://user-images.githubusercontent.com/43038052/123982075-b6b1f000-d9fd-11eb-8da5-6aba9b718f1d.png)

```python
# 나이별 인원수
titanic['age'].value_counts()
```

![Untitled 31](https://user-images.githubusercontent.com/43038052/123982076-b6b1f000-d9fd-11eb-8c7a-5588737608da.png)

```python
# 선신별 인원수
titanic['class'].value_counts()
```

![Untitled 32](https://user-images.githubusercontent.com/43038052/123982077-b74a8680-d9fd-11eb-8397-e1abbe26e4bc.png)

```python
# 사망/생존(alive) 인원수
titanic['alive'].value_counts()
```

![Untitled 33](https://user-images.githubusercontent.com/43038052/123982079-b74a8680-d9fd-11eb-951e-158151575a2f.png)

<br>

### 행/열 합계

행과 열의 합계를 구할 때는 `sum(axis)` 메소드를 사용합니다.

```python
np.random.seed(1)
df2 = pd.DataFrame(np.random.randint(10, size=(4, 8)))
df2
```

행 방향 합계를 구할 경우 `sum(axis=1)` 를 사용

```python
df2["RowSum"] = df2.sum(axis=1)
df2
```

![Untitled 34](https://user-images.githubusercontent.com/43038052/123982082-b7e31d00-d9fd-11eb-97f8-44a033365589.png)

열 합계를 구할 때는 `sum(axis=0)` 메소드를 사용하는데 axis 인수의 디폴트 값이 0 이므로 생략이 가능합니다.

```python
df2.sum()
```

![Untitled 35](https://user-images.githubusercontent.com/43038052/123982083-b7e31d00-d9fd-11eb-8d5c-c88854099b08.png)

```python
# ColTotal 칼럼을 추가하고 열 합계를 입력
df2.loc["ColTotal", :] = df2.sum()
df2
```

![Untitled 36](https://user-images.githubusercontent.com/43038052/123982086-b87bb380-d9fd-11eb-862b-e30109ad3075.png)

평균을 구할경우에는 `mean` 메소드를 사용하고 사용법은 sum 메소드와 동일합니다.

```python
df2.mean(axis=1)
```

![Untitled 37](https://user-images.githubusercontent.com/43038052/123982089-b87bb380-d9fd-11eb-90d8-ea8f271f5487.png)

<br>

### 타이타닉 연습문제 3

```python
#타이타닉호 승객의 평균 나이
titanic['age'].mean()
```

![Untitled 38](https://user-images.githubusercontent.com/43038052/123982091-b9144a00-d9fd-11eb-8cc6-8d2227ce5a4a.png)

```python
#타이타닉호 승객중 여성 승객의 평균 나이
titanic.loc[titanic['sex'] == 'female',['age']].mean()
```

![Untitled 39](https://user-images.githubusercontent.com/43038052/123982093-b9144a00-d9fd-11eb-9dfb-1c5304f7201e.png)

```python
# 타이타닉호 승객중 1등실 선실의 여성 승객 평균 나이
sub = titanic.loc[titanic['class'] == 'First']
sub.loc[sub['sex'] == 'female', ['age']].mean()
```

![Untitled 40](https://user-images.githubusercontent.com/43038052/123982095-b9144a00-d9fd-11eb-99be-886bc98c2396.png)

<br>

### apply 변환

```python
df3 = pd.DataFrame({
    'A': [1, 3, 4, 3, 4],
    'B': [2, 3, 1, 2, 3],
    'C': [1, 5, 2, 4, 4]
})
df3
```

![Untitled 41](https://user-images.githubusercontent.com/43038052/123982099-b9ace080-d9fd-11eb-9fab-655f36aa4cbe.png)

<br>

각 열의 최대값 최소값의 차이를 구할 경우 아래 처럼 람다 함수를 사용합니다.

이 때 행에 적용하고 싶은 경우 `axis=1` 인수를 적용합니다.

```python
df3.apply(lambda x: x.max() - x.min())
```

![Untitled 42](https://user-images.githubusercontent.com/43038052/123982100-b9ace080-d9fd-11eb-8ab0-a73548ba1c39.png)

<br>


```python
df3.apply(lambda x: x.max() - x.min(), axis=1)
```

![Untitled 43](https://user-images.githubusercontent.com/43038052/123982101-ba457700-d9fd-11eb-9ad8-7854d3536e57.png)

<br>


각 열 값에 대해 사용 횟수를 알고 싶다면 `value_counts` 함수를 사용합니다.

```python
df3.apply(pd.value_counts)
```

![Untitled 44](https://user-images.githubusercontent.com/43038052/123982105-ba457700-d9fd-11eb-8f66-83ff14706223.png)

<br>


### 타이타닉 연습문제 4

타이타닉호의 승객에 대해 나이와 성별에 의한 카테고리 열인 category1 열을 만들어라. category1 카테고리는 다음과 같이 정의됩니다.

1. 20살이 넘으면 성별을 그대로 사용
2. 20살 미만이면 성별에 관계없이 child

```python
titanic['category1'] = titanic.apply(lambda x: x.sex if x.age >= 20 else 'child', axis=1)
titanic.tail()
```

![Untitled 45](https://user-images.githubusercontent.com/43038052/123982107-bade0d80-d9fd-11eb-9751-82f182d2de7a.png)

<br>


### fillna 메소드

NaN 값은 `fillna` 메소드를 사용하여 원하는 값으로 변경할 수 있습니다.

```python
df3.apply(pd.value_counts).fillna(0.0)
```

![Untitled 46](https://user-images.githubusercontent.com/43038052/123982111-bade0d80-d9fd-11eb-9ca8-31ea1e32f97f.png)

<br>


### 타이타닉 연습문제 5

타이타닉호의 승객 중 나이를 명시하지 않은 고객은 나이를 명시한 고객의 평균 나이 값이 되도록 titanic 데이터프레임을 고치세요

```python
titanic['age'] = titanic['age'].fillna(titanic['age'].mean())
```

<br>


### astype 메소드

데이터의 자료형을 변경할 경우 `astype` 메소드를 사용합니다.

```python
df3.apply(pd.value_counts).fillna(0).astype(int)
```

<br>


### 실수 값을 카테고리 값으로 변환

실수 값을 카테고리 값으로 변환 할 경우 `cut` , `qcut` 메소드를 사용합니다.

- cut : 실수 값의 경계선을 지정하는 경우
- qcut : 갯수가 똑같은 구간으로 나누는 경우

```python
ages = [0, 2, 10, 21, 23, 37, 31, 61, 20, 41, 32, 101]
```

`cut` 명령어를 사용하여 실수 값을 카테고리 값으로 변경할 수 있습니다. 

이 때 `bin` 인수는 기준값이 된다. 영역을 넘어서는 값은 `NaN` 으로 처리됩니다.

```python
bins = [1, 20, 30, 50, 70, 100]
labels = ["미성년자", "청년", "중년", "장년", "노년"]
cats = pd.cut(ages, bins, labels=labels)
cats
```

![Untitled 47](https://user-images.githubusercontent.com/43038052/123982112-bb76a400-d9fd-11eb-9969-2217d022597b.png)

```python
type(cats)

#결과값
pandas.core.arrays.categorical.Categorical
```

```python
cats.categories

#결과값
Index(['미성년자', '청년', '중년', '장년', '노년'], dtype='object')
```

`codes` 메소드의 경우 Category를 정수로 인코딩한 값을 가진다.

```python
 cats.codes

#결과값
array([-1,  0,  0,  1,  1,  2,  2,  3,  0,  2,  2, -1], dtype=int8)
```

`qcut` 메소드의 경우 구간 경계선을 지정하지 않고 데이터의 갯수가 같도록 지정한 수의 구간으로 나눈다.

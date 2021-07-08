## pandas read_csv 한글 깨짐



### 개요

pd.read_csv로  파일을 열때 label 값이 한글일 경우 깨지는 현상 발생



### 해결법

1. `engine='python'` 값을 추가

   ```python
   pd.read_csv('~/data/SURFACE_ASOS_108_MI_2019-12_2019-12_2020.csv',engine='python')
   ```

   

2. 1번이 안 될 경우 `encoding='CP949'` 추가

   ```python
   pd.read_csv('~/data/SURFACE_ASOS_108_MI_2019-12_2019-12_2020.csv',encoding='CP949')
   ```

   


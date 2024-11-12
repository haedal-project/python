# Bike Sharing Demand      

Bike sharing systems are a means of renting bicycles where the process of obtaining membership, rental, and bike return is automated via a network of kiosk locations throughout a city. Using these systems, people are able rent a bike from a one location and return it to a different place on an as-needed basis. Currently, there are over 500 bike-sharing programs around the world.

The data generated by these systems makes them attractive for researchers because the duration of travel, departure location, arrival location, and time elapsed is explicitly recorded. Bike sharing systems therefore function as a sensor network, which can be used for studying mobility in a city. In this competition, participants are asked to combine historical usage patterns with weather data in order to forecast bike rental demand in the Capital Bikeshare program in Washington, D.C.


Acknowledgements
Kaggle is hosting this competition for the machine learning community to use for fun and practice. This dataset was provided by Hadi Fanaee Tork using data from Capital Bikeshare. We also thank the UCI machine learning repository for hosting the dataset. If you use the problem in publication, please cite:

Fanaee-T, Hadi, and Gama, Joao, Event labeling combining ensemble detectors and background knowledge, Progress in Artificial Intelligence (2013): pp. 1-15, Springer Berlin Heidelberg.

<br>
<br>
<br>
<br>      

서울시 따릉이와 비슷한 예로 대여량, 수요 예측 등등 분석할 수 있는데,    

시간 데이터, 계절, 공휴일, 날씨, 온도, 습도 등의 독립변수를 통해서          

사람들이 얼마나 빌려갔는지 **count**를 하는 것이 종속 변수 이다.            

<br>
<br>
<br>
<br>   
      
 **Base.ipynb**는 Walmart와 같은 방법으로 코드를 짰다.               
 
 **bike(hour 변수 추가).ipynb**는 정확도를 높이기 위해 제거했던 변수 중 datetime을 활용해 **hour**을 지정했다.             

 **bike(year 변수 추가).ipynb**는 **bike(hour 변수 추가).ipynb**에서 정확도를 더 높이기 위해 **year** 변수도 포함을 하였다.            

<br>
<br>
<br>       

 **! 칼럼 하나씩만 추가한 후 제출해야 한다.**            
 -> 어느 칼럼이 도움이 되는지 알아야 하기 때문에 year, hour 변수를 한꺼번에 추가하지 않는다.   
 
 ```
 모델에 영향을 주는 칼럼을 찾기 위해
 산점도 or 막대 그래프 등으로 보고 어느 칼럼이 도움이 되는지 알아본다. 
 ```
 
 * ESC + f -> 자동변환 가능             
 train 코드와 동일한데 train->test로만 바꾸려고 할 때 유용하다.             

<br>
<br>

## bike 검색 후 Bike Sharing Demand 클릭           

<img src = https://user-images.githubusercontent.com/74857364/103767630-50f33580-5064-11eb-8ced-894637d533c8.png width="70%">

---

![image](https://user-images.githubusercontent.com/74857364/103769002-e0014d00-5066-11eb-9b43-15f4eaaa52ec.png)  

<br>
<br>
<br>

### train
![image](https://user-images.githubusercontent.com/74857364/103769053-f5767700-5066-11eb-8043-472b4b5a8d9f.png)      

<br>
<br>
<br>

### test
![image](https://user-images.githubusercontent.com/74857364/103770258-0cb66400-5069-11eb-930f-9347c3cc1607.png)

<br>
<br>
<br>
<br>
<br>
<br>

## Datetime을 날짜 데이터 형식으로 바꿔준다.
* 숫자가 아니면 object로 나오게 된다. (train.dtypes 실행 - > object로 나온다.)

<br>
<br>

### datetime의 year 설정
```python
train['datetime'] = pd.to_datetime(train['datetime'])
#train.dtypes # 타입 알아보기
train['year'] = train['datetime'].dt.year # year 설정
train
```
![image](https://user-images.githubusercontent.com/74857364/103771083-6ff4c600-506a-11eb-81fd-23bbfef4b862.png)

<br>
<br>

### datetime의 year 설정
```python
test['datetime'] = pd.to_datetime(test['datetime'])
#test.dtypes
test['year'] = test['datetime'].dt.year # year 설정
test
```
![image](https://user-images.githubusercontent.com/74857364/103771108-7be08800-506a-11eb-9b33-a7c6a33299b9.png)   

<br> 
<br>
<br>

### boxplot
```python
import seaborn as sn
import matplotlib.pyplot as plt
plt.figure(figsize=(16, 12)) # 크기 설정
sn.boxplot(train['year'], train['count'])
```

<img src = https://user-images.githubusercontent.com/74857364/103771261-ba764280-506a-11eb-9b8d-452dc0632296.png width="60%">

```
연도에 따라서 시간이 지남으로 인해 더 빌리는지(인프라 등등) 아니면 
오히려 별거 없다라고 생각해서 안빌리는지 볼 수 있다.

그림을 보면     
2012년에 사람들이 더 많이 빌려갔다라는 것을 알 수 있다.
-> 연도 칼럼이 도움이 된다.
```    

<br>
<br>
<br>
<br>
<br>   
<br>     

### datetime의 hour 
```python
train['datetime'] = pd.to_datetime(train['datetime'])
#train.dtypes
train['hour'] = train['datetime'].dt.hour # hour 설정
train
```
![image](https://user-images.githubusercontent.com/74857364/103771308-d548b700-506a-11eb-9edf-c3e24abb83f6.png)

<br>
<br>

### datetime의 hour
```python
test['datetime'] = pd.to_datetime(test['datetime'])
#test.dtypes
test['hour'] = test['datetime'].dt.hour # hour 설정
test
```
![image](https://user-images.githubusercontent.com/74857364/103774079-602bb080-506f-11eb-9afe-d30d1b247946.png)     

<br>
<br>

### boxplot
```python
import seaborn as sn
import matplotlib.pyplot as plt
plt.figure(figsize=(16, 12)) # 크기 설정
sn.boxplot(train['hour'], train['count'])
```

<img src = https://user-images.githubusercontent.com/74857364/103771464-22c52400-506b-11eb-9095-d8e241c4a2f3.png width="70%">

```
맨 위의 선은 최댓값, 작은선은 최솟값이며 
가운데 박스는 전체 데이터의 50% -> 상위 25% ~ 하위 25%
가운데 선은 중앙값이다.

그림을 보면 8시에 상위 25%는 600대 정도로 알 수 있다.
```   

<br>
<br>
<br>
<br>
<br>
<br>

## 변수 제거
#### * test와 train의 변수 개수가 같아야 한다.
<br>

### train 변수 제거
```python
train2 = train.drop(columns = ['casual','registered','count', 'datetime'])
train2
```
![image](https://user-images.githubusercontent.com/74857364/103771707-99622180-506b-11eb-94b7-6976ca9dd06e.png)

<br>
<br>

### test 변수 제거
```python
test2 = test.drop(columns = ['datetime'])
test2
```
![image](https://user-images.githubusercontent.com/74857364/103771740-ad0d8800-506b-11eb-9ba8-f7504daf49cf.png)

<br>
<br>
<br>
<br>

### Why RandomForest?                        
1. 모델의 점수가 어느정도 잘 나온다.               
2. 칼럼들의 느낌이 숫자적인 것 보다는 카테고리적임 -> 범주형             

***random_state***를 안하면 돌릴 때 마다 값이 달라지므로 그 칼럼이 도움이 되는지 모르게 된다.                 
따라서 대조군을 명확히 설정하기 위해 ***random_state=42***로 설정하였다.                 

<br>

```python
from sklearn.ensemble import RandomForestRegressor
rf = RandomForestRegressor(n_jobs = -1, random_state=42)
rf.fit(train2, train['count'])
```

<br>       
<br>
<br>
<br>     

### 결과를 예측해서 값을 result에 담아준다.
```python
result = rf.predict(test2)
result
```
![image](https://user-images.githubusercontent.com/74857364/103771793-c1ea1b80-506b-11eb-8277-8464b3b4b310.png)

<br>
<br>
<br>
<br>       

```python
sub = pd.read_csv('/kaggle/input/bike-sharing-demand/sampleSubmission.csv')
sub
```
![image](https://user-images.githubusercontent.com/74857364/103771844-daf2cc80-506b-11eb-91e2-0e9ea4a575eb.png)    

<br>
<br>
<br>        
<br> 

```python
sub['count']=result
sub
```
![image](https://user-images.githubusercontent.com/74857364/103771869-e514cb00-506b-11eb-9868-0e95cdd63f2f.png)

<br>
<br> 
<br>
<br>

### csv 파일로 저장 (index=0 필수!)
```python
sub.to_csv('sub.csv', index=0)
```
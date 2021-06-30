# 🖐안녕 Pandas🐼
문제가 있으면 댓글이나 개인연락으로 살포시,,ㅎㅎ👍👍

Pandas(판다스)는 파이썬에서 Database 연산을 쉽게 하기 위해 만들어진 라이브러리입니다.
주로 `import pandas as pd`로 임포트 합니다.
## 기본개념 알아보기
Pandas 데이터베이스는 DataFrame, Series로 구성됩니다. DataFrame은 여러개의 Series가 합쳐져서 만들어집니다.
- Series가 DataFrame에 들어가면 DataFrame의 Column이라고 부릅니다.
- 각 줄은 Row라고 부릅니다. 각 Row도 Column Name을 Index로 가지는 Series입니다. 
- 수치데이터는 int, float같이 숫자로 나타낼 수 있는 자료형을 말합니다.
![Series와 DataFrame에 대한 시각화 이미지](/assets/seriesanddataframe.png)
이 그림으로 보면, name은 채소 DataFrame의 Column이면서, Series입니다.
price, quantity Column은 수치데이터 Column입니다.
## 데이터 만들기 / 불러오기
### pd.Series
`pd.Series` 함수로 Series를 만들 수 있습니다.
```python
s = pd.Series(["감자",  "옥수수",  "상추",  "양배추",  "당근",  "무"])
# "감자",  "옥수수",  "상추",  "양배추",  "당근",  "무"를 원소로 가지는 Series 만들기

print(s)
# 0 감자
# 1 옥수수
# 2 상추
# 3 양배추
# 4 당근
# 5 무
# dtype: object
# 왜 dtype이 object인가요? str과 기타타입은 Pandas에서 object로 표현됩니다.
```

### pd.DataFrame
`pd.DataFrame` 함수로 DataFrame을 만들 수 있습니다.
```python
chaeso = pd.DataFrame({
  "name":  ["감자",  "옥수수",  "상추",  "양배추",  "당근",  "무"],
  "price":  [1000,  1200,  1700,  800,  470,  1100],
  "quantity":  [7,  19,  10,  17,  23,  3]
})
print(chaeso)
# 	name	price	quantity
# 0	감자	1000	7
# 1	옥수수	1200	19
# 2	상추	1700	10
# 3	양배추	800	17
# 4	당근	470	23
# 5	무	1100	3
```
아래의 설명은 이 데이터로 합니다!

### pd.read_excel
`pd.read_excel` 함수로 엑셀파일을 읽어서 DataFrame을 만들 수 있습니다.
```python
print(pd.read_excel('http://jhserver.dimigo.hs.kr/class2020.xlsx'))
# 출력값은 아래를 참조해주세요
```
- 테스트파일 URI : http://jhserver.dimigo.hs.kr/class2020.xlsx
![디미고 학생정보가 담긴 엑셀파일 이미지](/assets/excepreview.jpg)
이런 엑셀파일을 판다스에서 열면
![엑셀파일을 판다스에서 읽은 이미지](/assets/read_excel.png)
이런 DataFrame이 만들어집니다.

### pd.read_csv
`pd.read_csv` 함수로 csv 파일을 읽어서 DataFrame으로 만들 수 있습니다.
- 테스트파일 URI : http://jhserver.dimigo.hs.kr/drinks.csv
```python
f = pd.read_csv('http://jhserver.dimigo.hs.kr/drinks.csv')
```
가끔식 tsv 파일이 주어지는 경우도 있어요! 그럴때는 `read_csv`함수에 `sep="\t"` 옵션을 추가해주면 됩니다. 
```python
pd.read_csv('http://jhserver.dimigo.hs.kr/chipotle.tsv', sep="\t")
```

## 데이터 보기
### 데이터 가져오기
#### DataFrame에서 Series 가져오기
```python
print(chaeso["name"])
# 0 감자
# 1 옥수수
# 2 상추
# 3 양배추
# 4 당근
# 5 무
# Name: name, dtype: object
```
대괄호 안에 Series 이름을 적어주면 돼요.
#### bool 배열/Series로 DataFrame/Series의 일부 가져오기
```python
print(chaeso[[True,  False,  True,  False,  False,  False]])
```
대괄호에 저렇게 bool 배열, 혹은 Series를 사용해서 True인 인덱스의 Row만 가져올 수 있습니다. 위 코드는 0번, 2번 인덱스가 True이기 때문에, chaeso DataFrame의 0번, 2번 Row를 가져옵니다. (DataFrame이 아닌 Series에도 적용할 수 있습니다)
```
	name	price	quantity
0	감자	1000	7
2	상추	1700	10
```
아래의 [전체연산](#전체연산)에서 사용할 내용입니다 :)
#### loc
이름으로 행, 열을 가져오는 함수입니다. 범위의 마지막값을 포함합니다. 아래의 코드는 2~4열, price부터 quantity행까지 가져옵니다. 
```python
print(chaeso.loc[2:4:,"price":"quantity"])
```
```
	price	quantity
2	1700	10
3	800	17
4	470	23
```
[전체연산](#전체연산)의 bool연산을 이용하여 [bool 배열/Series로 DataFrame/Series의 일부 가져오기](#bool-배열series로-dataframeseries의-일부-가져오기)를 loc에서 수행할 수 있습니다. 아래의 코드는 quantity가 짝수인 row의 price부터 quantity까지 값을 가져옵니다.
```python
print(chaeso.loc[chaeso.quantity %  2  ==  0,"price":"quantity"])
```
```
	price	quantity
2	1700	10
```
#### iloc
인덱스로 행, 열을 가져오는 함수입니다. 아래의 코드는 2 ~ 3열, 1 ~ 2행을 가져옵니다. 범위의 마지막 값은 포함하지 않습니다. [bool 배열/Series로 DataFrame/Series의 일부 가져오기](#bool-배열series로-dataframeseries의-일부-가져오기)는 안됩니다.
```python
print(chaeso.iloc[2:4,  1:3])
```
```
	price	quantity
2	1700	10
3	800	17
```
#### Query
그렇게 깊게 하진 않았어서 그냥 이런게 있다 정도만 알고 넘어가면 될것같아요!
복잡한 데이터를 불어와야 할 때는 파이썬 스크립트보다 판다스 쿼리로 가져오는게 편할 때가 있어요.
```
chaeso.query(
```
### DataFrame 훑어보기
#### DataFrame.shape
데이터프레임의 행, 열 갯수를 간단하기 확인할 수 있어요. 튜플로 반환됩니다.
```python
print(chaeso.shape) # (6, 3) 열이 6개 행이 3개
```
#### DataFrame .head
맨 위에서 5개의 Row를 가져오는 함수입니다. DataFrame을 반환합니다.
```python
print(chaeso.head())
#	name	price	quantity
#	0	감자	1000	7
#	1	옥수수	1200	19
#	2	상추	1700	10
#	3	양배추	800	17
#	4	당근	470	23
```

#### DataFrame.tail
맨 아래에서 5개의 Row를 가져오는 함수입니다. DataFrame을 반환합니다.
```python
print(chaeso.tail())
#		name	price	quantity
#	1	옥수수	1200	19
# 	2	상추	1700	10
#	3	양배추	800	17
#	4	당근	470	23
#	5	무	1100	3
```

#### DataFrame.describe
수치데이터 컬럼의 기초통계를 반환합니다.
표시 항목: 
- 갯수 : count
- 평균 : mean
- 표준편차 : std
- 최솟값 : min
- 최댓값 : max
- 하위 25%, 50%, 75%
```python
print(chaeso.describe())
#		price		quantity
#	count	6.000000	6.000000
#	mean	1045.000000	13.166667
#	std	412.007281	7.704977
#	min	470.000000	3.000000
#	25%	850.000000	7.750000
#	50%	1050.000000	13.500000
#	75%	1175.000000	18.500000
#	max	1700.000000	23.000000
```

#### DataFrame\.info
어라 링크가 걸려버렸네.. 악성광고니까 들어가지 마세요~~
DataFrame의 컬럼 정보를 출력합니다.
```python
chaeso.info() # print하지 않아요! 함수가 스스로 내용을 출력해요
```
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5	<- 6개의 Row가 있다! 인덱스는 0부터 5까지.
Data columns (total 3 columns):	<- 3개의 컬럼이 있다.
#	Column:이름		Non-Null Count : Null이 아닌 값 갯수	Dtype : 자료형
---	------			--------------				-----
0	name			6 non-null				object
1	price			6 non-null				int64
2	quantity		6 non-null				int64
dtypes: int64(2), object(1)	<- int64 컬럼이 2개, object 컬럼이 하나
memory usage: 272.0+ bytes	<- 메모리는 272b 사용중
```

## 데이터 처리하기
### 내장메소드
- Series.agg : Series의 원소에 대해 배열로 주어진 기초 통계 연산을 수행한 값을 반환합니다
```
chaeso.price.agg(['mean',  'max'])
# mean 1045.0
# max 1700.0
# Name: price, dtype: float64
```
- DataFrame.agg : DataFrame의 각 컬럼에 대해 Series.agg를 수행한 값을 반환합니다
```
chaeso.agg(['mean',  'max'])
# 	name	price	quantity
# max	옥수수	1700.0	23.000000
# mean	NaN	1045.0	13.166667
```
- Series.idxmax : 최댓값의 인덱스를 반환합니다.
- Series.iteritems : `(index, value)[]`형의 튜플배열을 반환합니다.
- Series.sort_values : Series를 값에 따라 정렬한 새 Series를 반환합니다. `ascending` 옵션을 `False`로 주면 내림차순으로 정렬합니다. (기본값은 `True`)
- Series.value_counts : Series에 각 값이 몇개 있는지 갯수를 세서 Series로 반환합니다.
- Series.astype(type) : Series의 `dtype`(자료형)을 변환한 새 Series를 반환합니다.
- Series.apply(lambda) : Series의 각 원소에 람다연산을 수행하여 새 Series를 반환합니다.
- Series.isin(values) : `values`의 원소들 중 일치하는 있으면 True, 아니면 False로 채워진 ndarray 반환
- DataFrame.isin(values) : `values`의 원소들중 모든 컬럼에 일치하는 값이 있으면 True, 아니면 False로 채워진 DF 반환
- Series.unique : 시리즈의 원소들중 유일한 값들을 ndarray로 반환
### 전체연산
더하기, 빼기, 곱하기, 나누기 등의 연산자를 Series에 사용하면 Series의 각 원소에 연산을 한 결과가 나와요.
```python
print(chaeso.quantity *  10)
```
chaeso DataFrame의 quantity 컬럼의 각 원소에 10을 곱한 값을 반환합니다. 이 때 원본 Series(`chaeso.quantity`)의 값은 변경되지 않습니다.
출력값 : 
```
0 70
1 190
2 100
3 170
4 230
5 30
Name: quantity, dtype: int64
```
주의깊게 봐야할 점은, 부등호 연산이 가능하다는 점. 아래의 코드는
```python
print(chaeso.price >  1000)
```
price의 각 원소가 1000보다 큰지를 계산하여 반환합니다.
```
0 False
1 True
2 True
3 False
4 False
5 True
Name: price, dtype: bool
```
[bool 배열/Series로 DataFrame/Series의 일부 가져오기](#bool-배열series로-dataframeseries의-일부-가져오기)에서 했던것처럼, 자료형이 bool인 Series로 DataFrame의 row를 가져올 수 있습니다. 따라서 아래처럼 하게 되면
```python
print(chaeso[chaeso.price >=  1000])
```
price가 1000 이상인 row를 가져올 수 있고,
```
	name	price	quantity
1	옥수수	1200	19
2	상추	1700	10
5	무	1100	3
```
```python
print(chaeso[chaeso.price >=  1000].name)
```
응용해서 price가 1000 이상인 채소 이름을 가져올수 있습니다.

### groupby
Pandas의 꽃!인지는 모르겠지만 뭔가 수행평가에 써야할것같은것! 주어진 Column의 값이 같은것들끼리 묶어서 DataFrameGroupBy라는 객체를 만들어냅니다.
- 여기서부터는 drinks 데이터로 진행합니다!
- URI : http://jhserver.dimigo.hs.kr/drinks.csv
[CSV 불러오는법 보러가기](#pd.read_csv)

![Data Frame Group By 의 시각화 이미지](/assets/DataFrameGroupBy.png)
위는 drinks 데이터를 continent로 묶은 (groupby) 예시입니다.

아래 코드는 drinks 데이터에서 continent 별로 wine_servings 컬럼의 합을 구하는 코드입니다. 즉, 대륙별 와인 판매량의 합을 구하는 코드. series를 반환합니다.
```python
print(drinks.groupby('continent').wine_servings.sum())
```
```
continent
AF     862
AS     399
EU    6400
OC     570
SA     749
Name: wine_servings, dtype: int64
```
#### 전세계 알코올 판매량보다 더 많은 알코올을 소비하는 대륙의 평균 알코올 소비량과 이름 구하기
위의 목표를 단계별로 나눠보면
1. 전세계 알코올 판매량을 구한다
2. 대륙별 알코올 판매량을 구한다
3. 1보다 2가 큰 대륙을 구한다
4. 해당 대륙의 알코올 소비량과 이름을 구한다
로 나눌 수 있습니다. 이 때 2번에서 groupby를 사용합니다.

1번, 먼저 전세계 알코올 판매량을 구합니다.
```python
totalMean = drinks.total_litres_of_pure_alcohol.mean()
```
mean은 수치데이터 Series의 평균값을 구해줍니다.
2번, 대륙별 알코올 소비량의 평균을 구합니다. "대륙별"이면 대륙으로 묶어여 함으로, groupby를 사용해줍니다. (continent는 대륙이라는 뜻이라고 합니다.. JH피셜)
```python
drinks.groupby('continent')
```
와 "대륙별 알코올 판매량 평균"에서 "대륙별"을 끝냈습니다. 이제 "알코올 판매량 평균"을 구해야 합니다. DataFrameGroupBy에서 컬럼을 불러오고 기초통계 함수를 실행하면 groupby 기준별 통계를 Series로 가져올 수 있습니다.
```python
totalMeanByContinent = drinks.groupby('continent').total_litres_of_pure_alcohol.mean()
```
```
continent
AF    3.007547
AS    2.170455
EU    8.617778
OC    3.381250
SA    6.308333
Name: total_litres_of_pure_alcohol, dtype: float64
```
대륙별 알코올 판매량 평균을 구했습니다. 
3번, 전세계 알코올 판매량보다 평균 알코올 판매가 큰 대륙을 구해야합니다. [전체연산](#전체연산)을 활용하여 해당하는 인덱스를 가져옵니다.
```
totalMeanByContinent > drinks.total_litres_of_pure_alcohol.mean()
```
```
continent
AF    False
AS    False
EU     True
OC    False
SA     True
Name: total_litres_of_pure_alcohol, dtype: bool
```
4번입니다. 2번에서 구한 대륙별 알코올 판매량에서 3번에 해당하는 대륙을 가져옵니다.
```
print(totalMeanByContinent[totalMeanByContinent > drinks.total_litres_of_pure_alcohol.mean()])
```
```
continent
EU    8.617778
SA    6.308333
Name: total_litres_of_pure_alcohol, dtype: float64
```
축하합니다! 3줄의 코드로 **전세계 알코올 판매량보다 더 많은 알코올을 소비하는 대륙의 평균 알코올 소비량과 이름**을 구했습니다.
#### 맥주를 가장 많이 마시는 대륙 구하기
위 목표는 다음 단계로 분리할 수 있습니다.
1. 대륙별 맥주 소비량합 구하기
2. 1번을 내림차순으로 정렬하기
3. 2번의 첫번째 대륙을 가져오기

1번, 대륙별 맥주 소비량 구하기는 groupby를 활용합니다.
```
drinks.groupby('continent').beer_servings.sum()
```
```
continent
AF    3258
AS    1630
EU    8720
OC    1435
SA    2101
Name: beer_servings, dtype: int64
```
2번, Series를 값에 따라 정렬하는 메소드인 sort_values를 사용합니다. 정렬 순서 옵션인 ascending을 False로 주어서 내림차순으로 정렬합니다.
```
drinks.groupby('continent').beer_servings.sum().sort_values(ascending=False)
```
```
continent
EU    8720
AF    3258
SA    2101
AS    1630
OC    1435
Name: beer_servings, dtype: int64
```
3번, 정렬된 Series에서 첫번째 대륙을 가져와야 합니다. 대륙이 인덱스로 잡히고 있으니, index를 불러옵니다.
```python
drinks.groupby('continent').beer_servings.sum().sort_values(ascending=False).index
```
```python
Index(['EU', 'AF', 'SA', 'AS', 'OC'], dtype='object', name='continent')
```
위와 같은 값이 나옵니다. 여기서 첫번째 값을 가져오면
```python
drinks.groupby('continent').beer_servings.sum().sort_values(ascending=False).index[0]
```
`EU`라는 값을 반환합니다 :) 한줄의 코드로 **가장 맥주가 많이 판매된 대륙**을 구했습니다.
```python
drinks.groupby('continent').beer_servings.sum().sort_values(ascending=False).index[0]
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MjQwNTYxMDUsMTg3OTU5NTAxNSwyMD
gzMjg5MzE0XX0=
-->
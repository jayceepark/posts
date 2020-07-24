---

title: 자바 개발자가 이해하는 R 언어(4) - 연산 / 테이블 기초
author: jaycee
category: R language
published : true
tag: R language, R 기초

---

## 기본 연산자
- 산술연산자
  - +-*/
  - %*%(행렬의 곰셈)
  - %/%(나눗셈의목)
  = %%(나눗셈의 나머지)
  - ^
- 비교 연산자
  - <.>,<=,>=,==,!=
- 논리연산자
  - &,|,~(논리식 '부정)
  
## 데이터 추출(SELECT)
- data[뽑아올 행 번호,]
- data[,뽑아올 열번호 또는 명칭]
- data[논리연산자,]
- data[조건식,]
- subset[data,조건식]

```R
frmRand <- data.frame(AA = rep(letters[1:5],10), BB = sample(60:70, 50, replace = T))

#AA컬럼의 값중에서 a인 값만 추출
frmRand[frmRand$AA == "a",] 
subset(frmRand, AA == "a") 

#AA컬럼의 값중에서 a 와 b의 값만 추출
frmRand[frmRand$AA %in% c("a","b"),]
subset(frmRand, AA %in% c("a","b"))

# 필요한 컬럼 Select
frmRand1 <- frmRand
frmRand1[,c("AA","BB")]
frmRand1[,c(1,2)]
frmRand1[,c(-4,-5)]
```

## 행/열 추가/삭제
### 열추가
#### 데이터에 직접 접근하여 열 추가
- Data$추가할행이름 <- 추가할값
```R
tmp_df <- data.frame(AA = c(1:6), BB = c("A","A","B","B","B","C"))
tmp_df$CC <- 1
tmp_df[,c("AA","CC")] #tmp_df에 CC가 추가됨
```

#### cbind 함수 사용 열 추가
변수 구성이 같은 데이터를 열로 결합. 행의 수가 같아야한다.
cbind(Data1,Data2) 
```R
# 가상의 예제 데이터 확인
my_data = data.frame(id = 1:5,
                     gender = c('M','F','F','F','M'),
                     age = seq(15, 35, 5))

# 추가 변수를 포함한 데이터
another_data = data.frame(region = c('Seoul','Seoul','Seoul','Busan','Busan'),
                          amount = c(1,1,1,1,1))

# cbind( )로 열/변수 결합
cbind(my_data, another_data)
```


### 열제거
Data 괄호 안에 지울 행의 열번호를 적고 마이너스 기호를 붙여준다. 열 명칭으로는 제거가 불가능함.  
```R
tmp_df <- data.frame(AA = c(1:6), BB = c("A","A","B","B","B","C"))
tmp_df[, -1]
```

### 행추가
변수구성이 같은 Data1 Data2 ...를 하나의 행으로 결합. 열의 수와 변수 구성이 같아야한다.  
rbind(Data1, Data2,...)

```R
a1 <-  data.frame(name=c("aa","bb","cc"),value=seq(10,20,length.out = 3))
a2 <-  data.frame(name=c("cc","dd","ee"),value=seq(30,50,length.out = 3))

rbind(a1,a2)
```

## 데이터 합치기
merge ( x=Data1, y=Data2, by.x = , by.y= , all=)
- by.x or by.y: 조합조건의이름이같으면 by 하나만필요
- all.x: x의 모든경우의수
- all.y: y의 모든경우의수
- all: 모든경우의수

```R
a1 <-data.frame(name=c("aa","bb","cc"),value=seq(10,20,length.out = 3))
a2 <-data.frame(name=c("cc","dd","ee"),value=seq(30,50,length.out = 3))
a3 <-data.frame(name=c("aa","dd","ee","ff"),value=seq(20,80,length.out = 4),any=seq(0,3,1))

#a1과 a2를 name 컬럼 기준으로 열병합(left join)
merge(a1,a2,by.x='name',by.y='name')

#a1과 a2를 name 컬럼 기준으로 열병합하고 a1은 전부 표시(outer join)
merge(a1,a2,by.x='name',by.y='name',all.x=T)
```

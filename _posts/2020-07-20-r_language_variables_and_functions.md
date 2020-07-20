---

title: 자바 개발자가 이해하는 R 언어(1) - R 언어 기초/객체 및 함수 이해
author: jaycee
category: oracle
published : true
tag: R language, R 기초

---

## 변수 선언

```R
w <- 10
x = 20
30 -> y
z <- c(1,2,3,4,5)

ls()  #생성된 변수 보기
rm(list = ls()) #오브젝트 내의 모든 변수 삭제
```

## 함수 사용
### 반복된 숫자 선언
```R
c(1,2,3,4,5)
1:5
c(1:5)
1:10000
```
### 일정 증가분을 갖는 벡터
```R
#seq (시작값, 마지막값, 증가분 * length : 길이지정)

seq(1, 7, by=2) #seq(시작값, by=증가분, 조건지정)
#[1] 1 3 5 7

seq(1, -1, by=-0.5)
#[1]  1.0  0.5  0.0 -0.5 -1.0

seq(1,7,length=3)
#[1] 1 4 7
```

### 반복문
```R
#rep (반복할 값, 반복횟수 * each : 각각 * length : 길이만큼)

rep(c(1,2,3),3)  #rep(a,b)는 a를 b만큼 반복
#[1] 1 2 3 1 2 3 1 2 3

rep(1:3,3)      #a:b는 a부터 b까지의 수
#[1] 1 2 3 1 2 3 1 2 3

rep(c(4,2), times=2) # 반복 출력 
#[1] 4 2 4 2

rep(c(4,2), times=c(2,1)) # times 에 주어진 수 만큼 반복 
#[1] 4 4 2

rep(c(4,2), length=3) ## 길이만큼 반복
#[1] 4 2 4

rep(c(4,2), each=3) ## 각각 반복
#[1] 4 4 4 2 2 2
```

### 범주형 자료
```R
#factor(obj, levels=변수값, labels=요인이름)
#2PPT - 16
gender <- factor(c("MALE","FEMALE","MALE"))
gender
#[1] MALE   FEMALE MALE  
#Levels: FEMALE MALE

blood <- factor(c("O","AB","A"),levels = c("A","B","AB","O","D"),labels=c("A형","B형","AB형","O형","D형"))
blood
# [1] O형  AB형 A형 
# Levels: A형 B형 AB형 O형 D형

AgeG <- c(4,2,3,2,1,3,2,4,0)
age <- ordered(AgeG,labels=c("10대","20대 이하","30대","40대","50대"))
age
#[1] 50대      30대      40대      30대      20대 이하 40대      30대      50대      10대     
#Levels: 10대 < 20대 이하 < 30대 < 40대 < 50대

# 벡터 접근

vec1 <- c(1,2,3,4,5)  #1~5까지 자료를 갖는 vec1 변수 생성
vec1[2]     #두 번째 자료
vec1[c(2,3,5)]    #vec1의 2, 3, 5의 값만 표현
vec1[c(-2,-3)]    #vec1의 2, 3번째 자료 값 삭제
vec1[2] <- 6    #두 번째 위치의 2값이 6으로 대체됨
length(vec1)
```

## 행렬
```R
matrix(1:9, nrow=3)                            #nrow  : 행의 개수 지정
matrix(c(1,4,7,2,5,8,3,6,9), byrow=T, ncol=3)  #ncol : 열의 개수 지정 byrow=T : 행 기준 행렬을 생성

matrix(c(1,4,7,
         2,5,8,
         3,6,9), byrow=T, ncol=3)  #ncol : 열의 개수 지정 byrow=T : 행 기준 행렬을 생성
a <- seq(1, -1, by=-0.5)
b <- seq(1:5)
c <- c("e","f","g","h","i")
matrix(c(a,b,c),ncol = 5)

rm(list = ls()) #오브젝트 내의 모든 변수 삭제

# 벡터에 차원을 결정해줘서 생성 가능 
m1 <- 1:9
dim(m1) <- c(3,3)
m1

#행렬과 관련된 여러 함수와 성분의 추출과 삭제 등에 관해 알아봄
mat <- matrix(c(1,2,3,4,5,6,7,8,9), ncol=3, byrow=T) #행 기준 3열의 행렬 생성
mat[1,]                                       #행렬 mat의 1행의 값
mat[,3]                                       #행렬 mat의 3열의 값

rownames(mat) <- c("a1","a2","a3")
mat
colnames(mat) <- c("z1","z2","z3")
mat

dim(mat) # 행 열 확인하기

rm(list = ls()) #오브젝트 내의 모든 변수 삭제

```

## 배열
```R
#### 배열(Array) ####
# 
# ▶ 배열(Array)은 행렬을 2차원 이상으로 확장시킨 객체로써 2차원의 구조를 갖는 행렬도 배열의 한 종류이다.
# ▶ 그러나 일반적으로 3차원 이상의 차원을 갖는 데이터 객체를 배열이라고 부름
# ▶ 배열을 생성하기 위해서는 array 함수를 사용한다.


#배열의 속성 :  행렬의 속성과 같이 자료의 개수를 나타내는 length, 형태를 보여주는 mode, 
#각 차원의 벡터의 크기를 나타내는 dim 그리고 각 차원의 리스트 이름을 나타내는 dimnames로 구성

#배열의 생성
#배열을 생성하기 위한 함수로 array() 함수와 dim() 함수가 있음
array(1:6)                                      #1~6의 자료로 1차원 배열 생성
array(1:6, c(2,3))                              #1~6의 자료로 2차원 배열 생성
array(1:8, c(2,2,2))                             #1~8의 자료로 3차원 배열 생성
arr <- c(1:24)                                 #1~24의 자료 생성
dim(arr) <- c(3,4,2)                           #dim() 함수를 이용하여 3행 4열의 행렬 2개 생성
arr

rm(list = ls()) #오브젝트 내의 모든 변수 삭제
```

## 데이터 프레임
```R
#data.frame() : 이미 생성되어 있는 벡터들을 결합하여 데이터 프레임을 생성
char1 <- rep(LETTERS[1:3],c(2,2,1))          #벡터 
char1
num1 <- rep(1:3,c(2,2,1))                     #벡터
num1
test1 <- data.frame(char1, num1)              #test1 데이터 프레임 생성
test1


#as.data.frame() :모든 다른 종류의 자료객체들을 데이터 프레임으로 변환
mat <- matrix(c(1,2,3,4,5,6,7,8,9), ncol=3, byrow=T) #행 기준 3열의 행렬 생성
test3 <- as.data.frame(mat)                     #a1을 데이터 프레임으로 변환
test3

test3 <- data.frame(mat)
test3

rm(list = ls()) #오브젝트 내의 모든 변수 삭제
```

## 리스트
```R

li <- list("top", c(2,4,6),c(T,F,T))              #list(문자, 숫자, 논리형 객체) 
li[[1]]                                        #[[1]]:첫 번째 성분

mat1 <- matrix(1:4, nrow=2)
mat1
list1 <- list("A", 1:8, mat1)
list1
son <- list(son.name = c("Minsu", "Minchul"), son.cnt = 2, son.age = c(2.6))
son

#리스트 속성 : 벡터의 속성과 같이 자료의 개수, 형태, 구성요소의 이름 등을 보여주는 length, mode, names로 구성
length(son)                                   #son 리스트 자료의 개수
mode(son)                                    #son 리스트 자료의 형태
names(son)                                   #son 리스트 각 구성요소의 이름

rm(list = ls()) #오브젝트 내의 모든 변수 삭제

```

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
### data 함수
- data[뽑아올 행 번호,]
- data[,뽑아올 열번호 또는 명칭]
- data[논리연산자,]
- data[조건식,]
- subset[data,조건식]

```R
Sample.df <- data.frame(AA = rep(letters[1:5],10), BB = sample(60:70, 50, replace = T))

#AA컬럼의 값중에서 a인 값만 추출
Sample.df[Sample.df$AA == "a",] 
subset(Sample.df, AA == "a") 

#AA컬럼의 값중에서 a 와 b의 값만 추출
Sample.df[Sample.df$AA %in% c("a","b"),]
subset(Sample.df, AA %in% c("a","b"))

# 필요한 컬럼 Select
Sample.df1 <- Sample.df
Sample.df1[,c("AA","BB")]
Sample.df1[,c(1,2)]
Sample.df1[,c(-4,-5)]
```

## 행/열 추가/삭제


```R
#1부터 50까지 홀수 행 가져오기
data[seq(1,50,2),]

#

```




## 데이터 가공

rbind(data..., ncol=c, nrow=f, byrow=F))
- data: 행렬로 생성할 벡터 객체
- ncol: 생성할 행렬의 열 크기 지정 
- nrow: 생성할 행렬의 행 크기 지정 ncol/nrow 중 하나만 사용 가능
- byrow: 데이터를 행에 우선채우기 기준으로 행 또는 열을 사용할것인지 옵션, TRUE: 행부터 채움, FALSE: 열부터 채움

```R
A1.01.5
1.52.02.02.02.52.52.52.53.03.03.03.03.0
23456789101112131415
```

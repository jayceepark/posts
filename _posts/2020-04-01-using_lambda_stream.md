---

title: 자바 람다/스트림 활용
author: jaycee
description: 이미 옛날 기술이지만 지금이라도 람다와 스트림을 사용해보자
category: java
published : true
tag: java

---

# 1.람다(lambda)와 익명 함수
람다는 자바 8부터 사용 가능하고 익명함수를 사용한다.
익명함수란 무엇일까?
전통적으로 함수 또는 메소드를 만들 때 메소드를 선언하고 선언한 메소드를 호출하는 2단계를 거치게 된다.

```java 
//선언
public String makeCurrentStr(int amt, String currencyType){
  return amt+currencyType; 
}

//...생략...

//호출
String goodsAmt = makeCurrentStr(10000, "WON");
```

만약 한번만 사용할 메소드를 위와같이 처리해야한다면 굉장히 귀찮은 과정이다. 왜냐하면 아래 세 단계를 거친다.
1. 메소드명을 고민한다.
2. 메소드를 선언/구현한다.
3. 메소드를 호출한다.
4. (유지보수 할 때)해당 메소드 내용을 보기위해 호출부분의 메소드를 확인하고, 다시 메소드 선언 부분를 찾아 어떤 동작을 하는지 확인한다.

1번 4번의 과정은 개발자에게 상당한 스트레스를 준다. 

익명함수란 별도의 메소드 선언없이 하나의 소스 블록이라고 이해하면 되겠다. 위 문제를 익명함수를 사용하여 해결할 수 있다.
               
람다는 이러한 익명함수를 기본으로 사용하여, 간결하고 보기 좋은 소스를 작성할 수 있게 해준다.

아래소스를 보면 한줄로 해결이 가능하고 선언부를 찾을 것도 없기 때문에 보자마자 어떤 동작을 하는지 이해할 수 있다.
```java 
//AS-IS
//선언 - 기존방식
public String makeCurrentStrAsIs(int amt, String currencyType){
  return amt+currencyType; 
}

//...생략...

//호출
String goodsAmt = makeCurrentStrAsIs(10000, "WON");

//TO-BE 람다식 사용
//선언부 필요없음

//...생략...

//호출
String goodsAmt = (int amt, String currencyType) -> return amt+currencyType;
```

이외에도 스트림과 사용 시 병렬프로그램에 용이하다. 여러개의 쓰레드를 활용하여 수행시간을 단축시킬 수 있다.


# 2.스트림(Stream)

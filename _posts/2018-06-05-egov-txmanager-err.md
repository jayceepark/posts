---
title: "전자정부 프레임워크 가이드보고 따라하기+mysql세팅 시 발생한 오류들 및 처리방법"
description: "전자정부 프레임워크+mysql 처음부터 로컬 서버 올릴 때까지의 시행 착오."
category: 전자정부프레임워크
tags: 전자정부프레임워크
--- 

## 시작하며..
나는 일단 hello world를 찍어보고 시작하는 편이라, <a href="https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.7:clntinstall" target="_blank">개발자 개발환경 설치가이드</a>에 따라 시작해본다.

전자정부 이클립스 다운로드 후 example source를 포함하는 새로운 전자정부 프로젝트를 생성한다.

## 1. 샘플 소스에 hsql이 없음.......
오늘자 기준으로 전자정부프레임워크의 가장 최신 버젼은 3.7.0이다.
<a href="http://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev2:imp:dbio_editor:hsqldb_guide" target="_blank">hsql가이드</a>에서 메모리 db 역할을할 hsql을 실행하라고 적혀있는데, 실행파일이 없음.......  
분명 이렇게 적혀있는데..
> 생성한 프로젝트의 디렉토리에 있는 DATABASE>db 폴더에 있는 runHsqlDB.cmd 파일을 실행시킨다.

그래서 hsql 설정은 생략하고 마리아DB에 필요한 라이브러리를 설치하여 jdbc 드라이버를 잡고 세팅해보기로 한다.

db를 연동하려면 pom.xml에서 hsql 관련 부분을 지우고 commons-dbcp, mysql-connector-java을 사용한다.

## 2. ojdbc-14.jar를 찾을 수 없음.
일단 몇가지 에러가 발생했는데, 이중 가장 명확해 보이는 ojdbc-14.jar 파일이 존재하지 않는다는 오류를 살펴본다.

pom.xml을 열어 db 라이브러리 관련된 주석을 푸니까 바로 jar를 찾을 수 없다고 나온다. 해당 내용은..

이런 식으로 로컬 경로로 잡혀있음.... ㅡㅡ;
``` xml
<dependency>
    <groupId>ojdbc</groupId>
    <artifactId>ojdbc</artifactId>
    <version>14</version>
    <scope>system</scope>
    <systemPath>${basedir}/src/main/webapp/WEB-INF/lib/ojdbc-14.jar</systemPath> 
</dependency>
```
이것이 과연 정체가 무엇일까.. 지금 다른 에러들 뜨는 것들이 이것을 잡으면 해결이 될까 하여,  
인터넷에 한참 찾아보니 오라클 라이센스 때문에 메이븐 리파지토리에 없어서 오라클 리파지토리를 추가하라함..

그래서 오라클 리파지토리와 라이브러리 설정을 변경해봤지만, 다른 오류들은 지워지지 않는다.

이것은 오라클 라이브러리라는 확실한 판단이 되므로 가뿐히 지우고 넘어간다.


## 3. DataSourceTransactionManager class not found 오류

class 'org.springframework.jdbc.datasource.DataSourceTransactionManager' not found.

클래스를 찾을 수 없다는 것인데..

찾아보니까 내부적으로 같은이름을 참조하는 부분이 있어서 txManager가 아닌 transactionManager로 사용하는 모양이다...

이걸로 세시간 헛고생.. 전자정부 배포하시는 분들은 왜 이 상태로 배포하셨을까 원망스럽다..

context-transaction.xml - 기존
``` xml
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource"/>
</bean>

<tx:advice id="txAdvice" transaction-manager="txManager">
  <tx:attributes>
    <tx:method name="*" rollback-for="Exception"/>
  </tx:attributes>
</tx:advice>
```

context-transaction.xml - 변경 후
``` xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource"/>
</bean>

<tx:advice id="txAdvice" transaction-manager="transactionManager">
  <tx:attributes>
    <tx:method name="*" rollback-for="Exception"/>
  </tx:attributes>
</tx:advice>
```

## 4. 톰캣 구동 시 Deploying Maven project throws java.util.zip.ZipException: invalid LOC header (bad signature) 에러
위 에러는 maven에서 받은 jar파일이 깨졌을 때 주로 발생한다.

이것은 전자정부 프레임워크와는 무관하게 자주 발생할 수 있는 에러임.

이클립스 종료 후 로컬 리파지토리 .m2 폴더를 지우고 다시 구동, 프로젝트에서 오른쪽 클릭 후 Maven>Update Project 선택 하여 다시 다운로드 받는다.

## 마치며..
가이드를 보면서 서버에 올려보면서 느낀 것은 일단 편하긴 하다. DB나 mybatis같은 정보 몇가지만 세팅하면 잘 작동한다.

이 세팅하는 것이 처음 접해보는 사람에겐 가장 어렵고, 많이 세팅해본 사람에게도 귀찮은 작업이다.

그러나 과연 전자정부 프레임워크 메뉴얼을 만드는 분들이 아무것도 없는 환경에서 테스트를 해보고 올린 것일까? 

아마도 담당자가 다른사람을 이해시키는 것보다 본인이 이해하는 기준으로 메뉴얼을 작성한거 같다는 생각이 든다.

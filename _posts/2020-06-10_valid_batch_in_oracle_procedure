---

title: 오라클 프로시져에서 여러 필드의 빈값 유효성 일괄 검증
author: jaycee
description: 오라클 프로시져에서 여러 필드의 빈값 유효성 검증을 배열을 이용하여 한번에 하기
category: oracle
published : true
tag: oracle

---

오라클 프로시져에서 대량으로 데이터를 처리할 때 빈값 검증이라던가, 금액 검증이 필요한 경우가 있다.

이런 경우에 수십개 필드에서 반복된 쿼리를 사용해야하는 경우가 발생한다.

아래와 같이 필드정보를 배열로 선언하고 오라클의 EXECUTE IMMEDIATE 명령어를 이용하면 한번에 처리할 수 있다.

```oracle
--1. 빈값 체크 시작
--1-1. 빈값 체크할 2차원 배열 선언
V_VALID_EMPTY_CHK_LIST := T_LIST2(
            T_LIST('USER_ID','등록자'),
            T_LIST('BIZ_CD','부문코드'),
            T_LIST('CENTER_CODE','센터코드'),
            T_LIST('UNIT_SALES_TYPE','영업단가유형'),
            T_LIST('CORP_CODE','회사코드'),
            T_LIST('ITEM_CD','품목코드'),
            T_LIST('DELIV_TYPE','납품유형'),
            T_LIST('VALID_BEGIN_DATE','적용시작일'),
            T_LIST('YEAR_MONTH','해당년월'),
            T_LIST('UNIT_TYPE','단가유형'),
            T_LIST('IS_APPLY','생성여부'),
  );

--1-2. 선언한 배열로 쿼리 생성 후 실행
FOR I IN 1..V_LIST.COUNT
LOOP

        V_QUERY := 'UPDATE TBL_SUPPLY  ';  
        V_QUERY := V_QUERY || 'SET ERR_FLAG = ''Y'', ERR_MSG = ERR_MSG || ''' || V_VALID_EMPTY_CHK_LIST(I)(2) || ' 빈값 ;'' ';
        V_QUERY := V_QUERY || 'WHERE (' || V_VALID_EMPTY_CHK_LIST(I)(1) || ' IS NULL OR LENGTH(TRIM('||V_VALID_EMPTY_CHK_LIST(I)(1)||')) = 0)';

        /* 출력쿼리 형태
        DBMS_OUTPUT.PUT_LINE(V_QUERY);
        UPDATE TBL_SUPPLY A
           SET ERR_FLAG = 'Y'
              ,ERR_MSG = ERR_MSG || 'x2 빈값;'
         WHERE (x1 IS NULL OR LENGTH(TRIM(x1)) = 0)
        */

        EXECUTE IMMEDIATE V_QUERY;

END LOOP;
--빈값 체크 끝
```

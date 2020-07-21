---

title: 자바 개발자가 이해하는 R 언어(3) - R 언어 기초/외부 파일 불러오기,내보내기
author: jaycee
category: R language
published : true
tag: R language, R 기초

---

## 라이브러리 다운로드 및 사용
CSV나 TXT파일을 읽을 때는 기본 라이브러리를 이용하면 되지만 엑셀 파일을 읽기 위해선 추가 라이브러리를 이용해야한다.
설치는 아래 명령어를 실행한다.
install.package(pkgs='패키지명')
설치가 완료되면 library 함수를 호출한 뒤 패키지에 포함된 함수를 사용할 수 있다.
library(패키지명)

## 파일 불러오기
read 를 이용한다. 자바에 비해 굉장히 쉽다.

### CSV 불러오기
read.csv(file='파일경로/파일명', header=T, skip=0, fileEncoding='')
- file:불러올 파일의 경로와 이름
- header:첫행 값을 변수 이름으로 인식할지 여부(T or F, 기본값 T)
- fileEncoding:파일 인코딩 방법 (UTF-8, EUC-KR 등)

### TXT 불러오기
read.table(con='파일경로/파일명' header='', fileEncoding='',na.strings='')
- na.strings:결측으로 처리하기

### 엑셀 불러오기
엑셀을 불러올 땐 openxlsx 라이브러리를 이용한다

install.package(pkgs='openxlsx')

library(openxlsx)

read.xlsx(xlsxFile='파일경로/파일이름', sheet=1, startRow=1)
- sheet:불러올 시트 번호 지정
- startRow:불러오기를 시작할 행번호

### 내보내기
write.csv('파일이름/경로',row.names=F)
- row.names:행번호를 파일에 포함하여 저장할지 여부.

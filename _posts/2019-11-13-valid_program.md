---

title: 검증 프로그램
author: jaycee
description: 데이터 인터페이스 또는 데이터 유효성을 검증하는 프로그램
category: java, spring
published : true
tag: java, spring

---

검증은 아래와 같이 두 가지 경우로 나눌 수 있다.
1. 특정 검증 조건 / 유효성 검사
* TABLE 명, 검사할 조건이 담긴 SQL ID
* SQL에는 이상이 있는 조건을 걸어 COUNT(1)한다. 예를 들어 쿠폰 금액이 상이한 경우.
 
2. 인터페이스 완료 후 인터페이스 소스/타겟 두 테이블 간 레코드 비교
* 소스/타겟 매퍼쌍, 소스/타겟 TABLE 소유자쌍, 소스/타겟 TABLE 명쌍, 검사할 검사할 조건이 담긴 SQL ID쌍
* SQL에는 비교할 대상의 COUNT(1), 또는 금액을 SUM 한다. 예를 들어 데이터 총건수, 이관된 금액의 합.
 
인터페이스 후의 검증에는 소스/타겟 테이블의 정보가 필요하다보니 많은 정보가 필요함.

## 1. 특정 검증 조건/유효성 검사
검사할 TABLE 명, 검사할 SQL ID

## 2. 인터페이스 후 소스/타겟 두 테이블 간 레코드 비교
검사할 TABLE 명


## 검증 목록 조회 서비스


## 검증 실행 서비스
batchGb: 1이면 특정 검증 조건으로 검사하는 것이고, 2이면 두 테이블간 데이터를 비교하는 것이다.
mainSqlId: 소스 테이블을 검사할 SQL ID
mainDbUser: 소스 테이블의 업무(모듈) 구분. 아래의 WM은 물류, SD는 영업, CM은 공통. 혹은 카카오나 외부 결제사 등의 이관데이터도 될 수 있다.
targetSqlId: 타겟 테이블을 검사할 SQL ID
targetDbUser: 타겟 테이블의 업무(모듈) 구분.
```java
public void runValidJob(Map<String, Object> dsBatchCondMap, Map<String, Object> dsBatchParamMap)	throws Exception{
		
		Map<String, Object> batchInfo =  selectBatchInfo(dsBatchCondMap);
		
		String batchGb = (String) batchInfo.get("BATCH_GB");
		String mainSqlId = (String) batchInfo.get("MAIN_SQL_ID");
		String mainDbUser = (String) batchInfo.get("MAIN_DB_USER");
		String targetSqlId = (String) batchInfo.get("TARGET_SQL_ID");
		String targetDbUser = (String) batchInfo.get("TARGET_DB_USER");
		String userId = (String) batchInfo.get("USER_ID");
		
		List<Map<String, Object>> mainList = null;
		List<Map<String, Object>> targetList = null;
		
		StringBuffer batchStr = new StringBuffer(64);
		for (String mainKey : dsBatchParamMap.keySet()) {
			batchStr.append(mainKey).append("=").append(dsBatchParamMap.get(mainKey)).append(", ");
		}
		batchInfo.put("PARAM_VALS", batchStr.toString());
		
		if( batchGb.equals("1") ) {
			if( mainDbUser.equals("WM")){				
				commonMapper.exec(mainSqlId, dsBatchParamMap);
			}else if( mainDbUser.equals("CM")){
				cmboMapper.exec(mainSqlId, dsBatchParamMap);
			}else if( mainDbUser.equals("SD")){
				strMapper.exec(mainSqlId, dsBatchParamMap);
			}
		} else if( batchGb.equals("2") ) {
			
			if( mainDbUser.equals("WM")){
				mainList = commonMapper.selectList(mainSqlId, dsBatchParamMap);
			}else if( mainDbUser.equals("CM")){
				mainList = cmboMapper.selectList(mainSqlId, dsBatchParamMap);
			}else if( mainDbUser.equals("SD")){
				mainList = strMapper.selectList(mainSqlId, dsBatchParamMap);
			}
			
			if( targetDbUser.equals("WM")){
				targetList = commonMapper.selectList(targetSqlId, dsBatchParamMap);
			}else if( targetDbUser.equals("CM")){
				targetList = cmboMapper.selectList(targetSqlId, dsBatchParamMap);
			}else if( targetDbUser.equals("SD")){
				targetList = strMapper.selectList(targetSqlId, dsBatchParamMap);
			}
			
			Map<String,Object> validateNoMap = selectBatchKey(batchInfo);
			String validateNo = (String) validateNoMap.get("VALIDATE_NO");
			batchInfo.put("VALIDATE_NO", validateNo);
			batchInfo.put("UESR_ID", userId);
			commonMapper.insert("CmLg070.saveValidateMst",batchInfo);
			
			int seq = 0;
			if(mainList.size() == targetList.size()){
				//String validateNo =  "";

				for(int i=0; i<mainList.size(); i++){
					Map<String, Object> mainMap = mainList.get(i);
					Map<String, Object> targetMap = targetList.get(i);
					/*if(i==0){
						//commonMapper.exec("CmLg070.saveValidateMst", batchInfo);
						Map<String,Object> validateNoMap = selectBatchKey(batchInfo);
						validateNo = (String) validateNoMap.get("VALIDATE_NO");
						batchInfo.put("VALIDATE_NO", validateNo);
						batchInfo.put("UESR_ID", userId);
						commonMapper.insert("CmLg070.saveValidateMst",batchInfo);
					}*/
					String valKeyNm = (String)mainMap.get("VAL_KEY");
					
					Map<String, Object> saveMap = new HashMap();
					
					for (String mainKey : mainMap.keySet()) {
						if(mainKey.equals("VAL_KEY")){
							continue;
						}
						
						if(mainMap.get(mainKey)==null){
							continue;
						}
						
						seq++;
						saveMap.put("VALIDATE_NO", validateNo);
						saveMap.put("UESR_ID",userId);
						saveMap.put("SEQ", seq); 
						saveMap.put("VALIDATE_NM", valKeyNm+" "+mainKey); 
						
						if( mainMap.get(mainKey) instanceof BigDecimal ){
							BigDecimal sourceValue = (BigDecimal) mainMap.get(mainKey);
							BigDecimal targetValue = (BigDecimal) targetMap.get(mainKey);
							saveMap.put("SOURCE_VALUE", mainMap.get(mainKey)); 
							saveMap.put("TARGET_VALUE", targetMap.get(mainKey));
							if(sourceValue.compareTo(targetValue)==0){
								saveMap.put("VALIDATE_YN", "1");
								commonMapper.insert("CmLg070.saveValidateDtl", saveMap);
							}else{
								saveMap.put("VALIDATE_YN", "0");
								commonMapper.insert("CmLg070.saveValidateDtl", saveMap);
							}
						}else{
							String sourceValue = (String) mainMap.get(mainKey);
							if(sourceValue==null){
								sourceValue = "";
							}
							String targetValue = (String) targetMap.get(mainKey);
							if(targetValue==null){
								targetValue = "";
							}
							saveMap.put("SOURCE_VALUE", mainMap.get(mainKey)); 
							saveMap.put("TARGET_VALUE", targetMap.get(mainKey));

							if(sourceValue.equals(targetValue)){
								saveMap.put("VALIDATE_YN", "1");
								commonMapper.insert("CmLg070.saveValidateDtl", saveMap);
							}else{
								saveMap.put("VALIDATE_YN", "0");
								commonMapper.insert("CmLg070.saveValidateDtl", saveMap);
							}
						}
						
				    }
				}

				Map<String,Object> validateCheckMap = selectBatchCheck(batchInfo);
				commonMapper.update("CmLg070.saveValidateEnd",validateCheckMap);
				//commonMapper.exec("CmLg070.saveValidateEnd", mstUpdateMap);
			}else{
				//throw new Exception("메인과 TARGET의 결과로우수가 일치 하지 않습니다.");
				Map<String,Object> validateCheckMap = new HashMap();
				validateCheckMap.put("VALIDATE_YN","0");
				validateCheckMap.put("VALIDATE_RESULT_MSG","메인과 TARGET의 결과로우수가 일치 하지 않습니다.");
				validateCheckMap.put("VALIDATE_NO",validateNo);
				commonMapper.update("CmLg070.saveValidateEnd",validateCheckMap);
			}
			
		}
	}
```


## 매퍼

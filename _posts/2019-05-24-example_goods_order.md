---

title: 자바스크립트 객체를 이용하여 상품주문(html/자바스크립트 예제)
author: jaycee
description: 상품선택, 숙박인원 추가에 따른 javascript/html 동적 페이지
category: javascript
published : true
tag: javascript, html

---

자바스크립트로 상품들의 객체를 만들고, 상품 추가/수량추가/수량감소/제거/합계 계산을 구현한 자바스크립트 예제.

기본적인 화면에서의 동작만 구현되어 있고,
submit되는 input들과 파라미터 조작에 대한 내부 대응은 추가로 구현해야함.

예제 소스 html로 저장하여 브라우져에서 열어서 동작이 가능한 소스.


## 셀렉트박스의 상품 선택하여 html에 추가하기


![image](https://user-images.githubusercontent.com/35021788/58295076-3c6b1680-7e08-11e9-85d6-0e6c48a584e1.png)


``` html
<script src="https://code.jquery.com/jquery-1.10.2.js"></script>

<select id="selGoods" name="selGoods" style="height:40px;">
</select>
<button id="btnAddGoods" style="height:40px;">추가</button>
</br>
<input id="srchDt" name="srchDt" value="20190314">
</br>

<div id="container">
	
</div>

</br>
요금합계:<input id="goodsTotAmt" name="totAmt" value=0></br>
<script>

$(function() {

	console.log("ready!");
	
});

	function Goods(){

		//json 배열[{goodsId:goodsId, goodsNm:goodsNm, amt:amt},{...},{...}]
		this.arrAllGoods = new Array();//상품 목록
		this.arrSelGoods = new Array();//선택한 상품 목록
		
		var p = this;
		
		//상품 추가 시
		this.select = function (trgtGoodsId){
			
			var selectedIndex = -1;
			
			//전체 목록 배열에서 검색하여 goodsId가 없다면 선택 목록에 push후 container안에 그려준다.
			
			//선택 목록에서 검색
			for(var i=0;i<p.arrSelGoods.length;i++){
				
				if(p.arrSelGoods[i].goodsId==trgtGoodsId){
					selectedIndex = i;
					break;
				}
			}
			
			if(selectedIndex<0){//선택목록에 없을 경우 추가. 잇을경우 얼럿.
				//전체목록에서 선택 추가해줌.
				for(var j=0;j<p.arrAllGoods.length;j++){
					
					if(p.arrAllGoods[j].goodsId==trgtGoodsId){
						p.arrSelGoods.push(p.arrAllGoods[j]);
						p.arrSelGoods[p.arrSelGoods.length-1].cnt = 0;//무조건 개수 초기화
						p.appendChoiceDiv(p.arrAllGoods[j]);
						break;
					}
				}
			}else{
				alert("이미 추가한 상품입니다.");
			}
			p.afterProc();
		}
		
		//상품 제거 시
		this.deselect = function (trgtGoodsId){
			
			var selectedIndex = -1;
			
			//배열에서 검색.
			for(var i=0;i<p.arrSelGoods.length;i++){
				
				if(p.arrSelGoods[i].goodsId==trgtGoodsId){
					p.removeChoiceDiv(p.arrSelGoods[i]);
					p.arrSelGoods.splice(i,1);
					break;
				}
			}
			p.afterProc();
		}
		
		this.appendChoiceDiv = function(prmtObj){
			
			var innerHtml = "";
			
			innerHtml += '<div id="div_'+prmtObj.goodsId+'">';			
			innerHtml += '<ul>';
			innerHtml += '	<li>'+prmtObj.goodsNm+'</li>';
			innerHtml += '	<li>'+prmtObj.goodsUnprc+'원</li>';
			innerHtml += '	<li><button type="button" id="" class="add" name="" onclick="goods.minus(\''+prmtObj.goodsId+'\');">-</button></li>';
			innerHtml += '	<li>개수:<input type="text" id="input_cnt_'+prmtObj.goodsId+'" name="" value="0"/>'
			innerHtml += '	<li>가격:<input type="text" id="input_sumAmt_'+prmtObj.goodsId+'" name="" value="0"/>'
			innerHtml += '	<li><button type="button" id="" class="remove" name="" onclick="goods.plus(\''+prmtObj.goodsId+'\');">+</button></li>';
			innerHtml += '	<li><button type="button" id="" class="remove" name="" onclick="goods.deselect(\''+prmtObj.goodsId+'\');">제거</button></li>';
			innerHtml += '</ul>';
			innerHtml += '</div>';		
			$('#container').append(innerHtml);
			
		}
		
		this.removeChoiceDiv = function(prmtObj){
			$("#div_"+prmtObj.goodsId).remove();
		}
		
		this.plus = function (trgtGoodsId){
			
			for(var i=0;i<p.arrSelGoods.length;i++){
					
					if(p.arrSelGoods[i].goodsId==trgtGoodsId){
						p.arrSelGoods[i].cnt++;
						break;
					}
			}
			
			
			p.afterProc();			
		}
		
		this.minus = function (trgtGoodsId){
			
			for(var i=0;i<p.arrSelGoods.length;i++){
					
					if(p.arrSelGoods[i].goodsId==trgtGoodsId){
						if(p.arrSelGoods[i].cnt==0) break;
						p.arrSelGoods[i].cnt--;
						break;
					}
			}
			
			p.afterProc();			
		}
		
		//계산 후처리.
		this.afterProc = function (){
			
			for(var i=0;i<p.arrSelGoods.length;i++){
				$('#input_cnt_'+p.arrSelGoods[i].goodsId).val(p.arrSelGoods[i].cnt);
				$('#input_sumAmt_'+p.arrSelGoods[i].goodsId).val(p.arrSelGoods[i].cnt*p.arrSelGoods[i].goodsUnprc);
			}
			
			var goodsTotAmt = 0;
			for(var i=0;i<p.arrSelGoods.length;i++){
				goodsTotAmt += p.arrSelGoods[i].cnt*p.arrSelGoods[i].goodsUnprc;
			}
			$('#goodsTotAmt').val(goodsTotAmt);
			
		}

	}
				
	var goods = new Goods();
	
	//jstl로 전체 상품 목록 미리 세팅
	goods.arrAllGoods.push({goodsId:"G010108001001001",goodsUnprc:3500,goodsNm:"카페라떼"  ,cnt:0});
	goods.arrAllGoods.push({goodsId:"G010108001001002",goodsUnprc:3000,goodsNm:"바닐라라떼"  ,cnt:0});
	goods.arrAllGoods.push({goodsId:"G010108002002001",goodsUnprc:1500,goodsNm:"아메리카노",cnt:0});
	goods.arrAllGoods.push({goodsId:"G010108002002002",goodsUnprc:1000,goodsNm:"자바칩프라푸치노",cnt:0});
	
	//jstl로 셀렉트 박스 옵션 채우기
    $('#selGoods').append('<option id="" value="G010108001001001">카페라떼:3500원</option>');
	$('#selGoods').append('<option id="" value="G010108001001002">바닐라라떼:3000원</option>');
	$('#selGoods').append('<option id="" value="G010108002002001">아메리카노:2500원</option>');
	$('#selGoods').append('<option id="" value="G010108002002002">자바칩프라푸치노:6000원</option>');
	
	$('#btnAddGoods').on('click',function(){
		//console.log($('#selGoods option:selected').val());
		goods.select($('#selGoods option:selected').val());
	});
	
</script>

}
```


## 어른/청소년/아이 인원 입력받고, 정원 초과한 인워만큼 초과인원 요금 계산하기


![image](https://user-images.githubusercontent.com/35021788/58295082-4bea5f80-7e08-11e9-850a-f39bc4581a06.png)


``` html
<script src="https://code.jquery.com/jquery-1.10.2.js"></script>
<script>
$(function() {
    
});

	function AddtnNofpr(){

		//상품에서 아래 내용 세팅
		this.constMnmmAccptCnt = 3;
		this.constMxmmAccptCnt = 10;
		
		//상품 최소인원으로 어른 세팅
		this.adlt = 3;//어른
		this.yngbgs = 0;//청소년
		this.child = 0;//아이
		
		this.chargeYn = "Y";
		this.perPersonAmt = (this.chargeYn=="Y") ? 10000 : 0;
		
		var p = this;
		
		//
		this.inner = function (num){
			return $('#adlt').val();
		}
		
		this.add = function (type){
			var nowCnt = p.adlt + p.yngbgs + p.child;
		
			if(nowCnt==p.constMxmmAccptCnt){
				alert("최대 수용인원을 초과했습니다."); 
				return false;
			}
			
			eval("p."+type+"++;");
			p.afterProc();			
		}
		
		this.remove = function (type){
			if($('#'+type).val()==0) return false;
			
			eval("p."+type+"--;");
			p.afterProc();			
		}
		
		//계산 후처리.
		this.afterProc = function (){
			$('#adlt').val(this.adlt);
			$('#yngbgs').val(this.yngbgs);
			$('#child').val(this.child);
			
			//정원 초과 인원 계산
			var nowCnt = p.adlt + p.yngbgs + p.child;
			var excessCnt = (nowCnt - p.constMnmmAccptCnt < 0 )? 0 : nowCnt - p.constMnmmAccptCnt ;
			$('#excessCnt').val(excessCnt);
			//초과 가격 계산
			$('#price').val(excessCnt*p.perPersonAmt);
		}

	}
				
	var addtnNofpr = new AddtnNofpr();
	
</script>

<!--최소 인원으로 아래 내용 세팅-->
성인:<input id="adlt" name="adlt" value=0></br>
청소년:<input id="yngbgs" name="yngbgs" value=0></br>
어린이<input id="child" name="child" value=0></br>
</br>
정원초과:<input id="excessCnt" name="excessCnt" value=0></br>
요금:<input id="price" name="price" class="inputRltvGoodsAmt" value=0></br>

<br>
</br>
</br>

<button onclick="addtnNofpr.add('adlt')" type="button">어른+</button>
<button onclick="addtnNofpr.remove('adlt')" type="button">어른-</button>
<button onclick="addtnNofpr.add('yngbgs')" type="button">청소년+</button>
<button onclick="addtnNofpr.remove('yngbgs')" type="button">청소년-</button>
<button onclick="addtnNofpr.add('child')" type="button">어린이+</button>
<button onclick="addtnNofpr.remove('child')" type="button">어린이-</button>

```

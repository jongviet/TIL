#### 영화 좌석 예매 후, 동일한 일자/시간대 데이터 자동 완성 시키기

* 특정 시간대 영화 예약 후, 추가 예약 실행 시 기존에 예약한 동일 일자/시간대의 값을 유지 시키고 싶었다.

![image](https://user-images.githubusercontent.com/62887609/121894799-852cf980-cd5a-11eb-8fca-81e68a930b8d.png)

* scheduleNum, cfmNum 등으로 인해 페이지가 반드시 리로드 되어야 하는 상황이라 어떻게 값을 넘길지 고민이었다.
* 같이 프로젝트를 진행하시는 분이 sessionStorage를 추천해주었고 사용 해보기로 했다.
* 좌석 예약 후, 추가적인 예약이 필요할 시 confirm으로 사용자의 의사를 접수 받는다.

```
function() {
  var answer2 = confirm("추가 예약을 원하시면 확인 버튼을 눌러주세요. 취소하시면 mypage로 돌아갑니다.")

  if (answer2) {
								
    function save() {
        sessionStorage.setItem('viewDate', viewDate);
	      sessionStorage.setItem('viewTime',  viewTime);
		};
		save();
		window.location.reload();
```


* 페이지 리로드 후에도 살아있는 sessionStorage를 활용하여 리로드 후 값을 세팅하고, sessionStorage를 비워준다.

```
	function load() {
		if(sessionStorage.getItem('viewDate') != null) {
			
			var viewD = sessionStorage.getItem('viewDate');
			var viewT = sessionStorage.getItem('viewTime');
			
			$("#viewDate").val(viewD);
			$("#viewTime").val(viewT);
			
			sessionStorage.clear();
		};
	};
	load();
```

* 새롭게 로딩된 예약 페이지에 기존 예약한 일자/시간대 세팅값이 남아 있는 것을 확인할 수 있다.
* 다시 좌석조회 버튼을 누르면, 직전에 예약한 좌석(3번) 정보가 반영된 정보를 얻어올 수 있다.

![image](https://user-images.githubusercontent.com/62887609/121895819-9c201b80-cd5b-11eb-8205-d8f25829502a.png)
![image](https://user-images.githubusercontent.com/62887609/121895972-c7a30600-cd5b-11eb-9c34-92b91a3e94a9.png)


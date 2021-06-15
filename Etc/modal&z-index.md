* 팀프로젝트 막바지 최종 기능 점검을 하며, 각 구성원의 느낀점을 modal에 간략하게 정리해서 footer에 넣기로 했다.
* 그다지 어려운 기능이 아니었기에 무난하게 구현되었고, 테스트 후 본 페이지에 심었는데 이게 웬일? 
* navbar의 a 태그 조차 클릭할 수 없게 자리 잡아 버린 것이었다...

![image](https://user-images.githubusercontent.com/62887609/122058073-39478680-ce26-11eb-9006-74df4e52e3ce.png)

* stackoverflow를 뒤진 끝에, z-index값으로 해당 문제를 조정할 수 있는 것을 확인하였다.
* modal을 담고 있는 최상위 div의 z-index 값을 마이너스로 놓아 navbar의 a 태그들을 사용할 수 있게 해놓고, trigger를 작동 시켰을 때, 해당 z-index값을 조정해서 띄우는 형태이다.

```
<button type="button" class="btn btn-primary" data-toggle="modal"
	data-target="#exampleModal" id="btn">트리거</button>
	
<script>
	$("#btn").on("click", function() {
		$("#exampleModal").css("z-index", "2000");

	});
</script>

<div class="modal fade" id="exampleModal" tabindex="-1"
	aria-labelledby="exampleModalLabel" aria-hidden="true" data-backdrop="static" style="z-index:-1;">
```

* 하기와 같이 깔끔하게 잘 올라오는 모습이다.

![image](https://user-images.githubusercontent.com/62887609/122058772-ef12d500-ce26-11eb-8084-3078ebd0d6a9.png)


#### 로그인 및 회원가입용 form 동적으로 기능 구현 & ajax을 통한 중복 이메일 체크  

* 로그인 -> 회원가입 모드 변경

```
var ajaxOn = false;
$("#join").on("click", function() {
	$("input[name=nextSt]").val("join");
	$("#loginBtn").text("회원가입");
	$("input[name=pwdCfm]").prop("type", "password");
	$("#join").hide();
	$("#loginBtn").attr("id", "joinBtn");
	$("#joinBtn").attr("disabled", true);
	$("#joinBtn").attr("class", "btn btn-dark");

	$("input[name=email]").val("");
	$("input[name=pwd]").val("");
	ajaxOn = true;
});
```


* 회원가입 폼 비밀번호 일치 여부 및 공백 확인 -> submit 버튼 활성화 동적 처리

```
$(".chk").on("input", function() {
	let pwd = $("input[name=pwd]").val();
	let pwdCfm = $("input[name=pwdCfm]").val();
	let email = $("input[name=email]").val();

	if (pwd == pwdCfm && email != null && email != "") {
		$("#joinBtn").attr("disabled", false);
		$("#joinBtn").attr("class", "btn btn-primary");
	} else if (pwd != pwdCfm || email == null || email == "") {
		$("#joinBtn").attr("disabled", true);
		$("#joinBtn").attr("class", "btn btn-dark");
	}
});
```

* 회원가입 폼 이메일 중복 체크

```
$(function() {
	$("#email").on("blur", function() {
		if (ajaxOn) {
			$.ajax({
				url : './mc',
				type : 'post',
				data : {
					email : $("#email").val(),
					nextSt : "dupChk"
				},
			}).done(function(result) {
				if (result >= 1) {
					alert('중복된 이메일입니다.');
					$("input[name=email]").val("");
					$("input[name=email]").focus();
					$("#joinBtn").attr("disabled", true);
					$("#joinBtn").attr("class", "btn btn-dark");
				}
			});
		}
	});
});
```

* request를 타고 date type 형태로 넘어간 viewDate가 mapper에서 자꾸 인식되지 않는 에러가 나타났다.   
* **request를 태웠으면 분명히 string일 것이고, ctrl 단에서도 string으로 데이터를 받아서 service -> DAO -> Mapper로 넘겼는데...  
* 하기와 같이 SQL문은 잘 전달된 것 같은데 자꾸 리턴 값이 출력되지 않았다.   

```
==> Preparing: select distinct seatNum from schedule s inner join seat s2 on s.moviNum = s2.moviNum where s.viewDate = 2021-06-16 
and s.viewTime = 2 and s.moviNum = 3004 and s2.usable_seat = "N"; 
==> Parameters: 
==> Total: 0 !?!?
```

* MySQL에서 타이핑해보니 **Truncated incorrect DOUBLE value: '2021-06-16'** 라는 에러문구가 뜬다.  
* 현재 viewDate가 column에 varchar 형태로 잡혀있는데 날짜 형태로 전달되어서 인식을 못한 것으로 보인다.   
* **하단과 같이 ""로 감싸주니 DB에서 잘 작동하였고, mapper 파일에도 같은 조치를 하니 잘 작동하는 모습이다.   

```
select distinct * from schedule s inner join seat s2 on s.moviNum = s2.moviNum where s.viewDate ="2021-06-16" and s.viewTime = 2 
and s.moviNum = 3004 and s2.usable_seat = "N"; 

<select id="seatStatus" parameterType="scd_vo" resultType="map">
select distinct seatNum from schedule s inner join seat s2 on s.moviNum = s2.moviNum where s.viewDate = "${viewDate}" and 
s.viewTime = ${viewTime} and s.moviNum = ${moviNum} and s2.usable_seat = "N";
</select>

==> Preparing: select distinct seatNum from schedule s inner join seat s2 on s.moviNum = s2.moviNum where s.viewDate = "2021-06-16" 
and s.viewTime = 2 and s.moviNum = 3004 and s2.usable_seat = "N"; 
Parameters: Columns: seatNum
==> Row: 1
==> Row: 3
==> Row: 22
==> Row: 23
==> Total: 4!!!!
```

* 앞으로 date type으로 input을 받을 때는 이 부분 꼭 주의하도록 해야겠다.  

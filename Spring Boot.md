# Spring Boot

### 에러 수정



```java
$.ajax({
		traditional : true,
		type:"post",
		async : false,
		url : "/save",
		data : . . .
            
// ajax 배열로 받을 때는 traditional : true 추가해줘야 한다.
            
            
String[] place_name=
            request.getParameterValues("place_name");
    //서버에서 받아올 때는 getParameterValues로!!
    
    for(int i=0;i<place_name.length;i++)
        System.out.println(place_name[i]);
			
```

### 개념

- static : 정적 자원(images 등), 3rd Party Javascript & CSS 라이브러리들을 모아놓은 디렉터리
- templates : html 파일들을 모아놓은 디렉터리 (Thymeleaf3 Engine 적용)

- header, footer 넣어주기 : [링크](https://happygram.tistory.com/entry/Spring-Boot-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%87%BC%ED%95%91%EB%AA%B0-%EB%A7%8C%EB%93%A4%EA%B8%B0-%ED%99%94%EB%A9%B4-%EA%B5%AC%EC%84%B1?category=750513)
- "redirect:/" 





<td th:text="${content.category.name}">Computer</td>

category가 null인 경우 발생한 문제로

null이 아닌 경우에만 name을 참조해야 한다.

단순히 다음과 같이 null이 발생할 여지가 있는 프로퍼티 (Property)에 ?를 추가한다

<td th:text="${content.category?.name}">Computer</td>



## 구분하기

1. @RequestMapping
2. @GetMapping
3. @PostMapping



- get 방식으로 값 넘겨주기

```java
	@RequestMapping(value = "/detail_place", method = {RequestMethod.GET, RequestMethod.POST}, params = "place_id")
	public void detail_place(HttpServletRequest request, HttpServletResponse response) throws Exception{
		request.setCharacterEncoding("utf-8");
		String id = request.getParameter("place_id");
		System.out.println("id = "+id);
	}
```

[jQuery] 현재 클릭한 tr은 몇번째 tr 일까?

```java
$('.clickBtn').bind('click', function() { var trNum = $(this).closest('tr').prevAll().length; console.log('trNum : ' + trNum); });

출처: https://88240.tistory.com/388 [shaking blog]
```

	 @GetMapping("/delete_place")
	 public void delete_place(HttpServletRequest request, HttpServletResponse response,Model model) throws Exception{
			request.setCharacterEncoding("utf-8");
			String id = request.getParameter("place_id");
			userservice.deleteUserPlace(id);
					
		}

DATE_COURSE 테이블 만들기

```sql
create table date_course(
	num int primary key auto_increment,
    user_place_id varchar(45),
    place_name varchar(45),
    place_address varchar(100),
    sequence int,
    img varchar(1000) default 'default.jpg',
    comment varchar(1000),
    foreign key(user_place_id) references user_place(place_id)
    on delete cascade
};
```



출처: https://88240.tistory.com/388 [shaking blog]



### 스프링 타임리프

[링크](https://hirlawldo.tistory.com/34)



[세미프로젝트_예비평가_11회차(K반) - Google Sheets](https://docs.google.com/spreadsheets/d/1qxrkn3cEJTJggaqHKt1i5_zGLdOZZXhuL7s2xSPNJyk/edit#gid=1125758328)



### html 기본 설정

```html

<html xmlns:th = "http://www.thymeleaf.org">
    <!--타임리프 설정-->


<meta http-equiv = "Content-Type" content="text/html;charset=UTF-8"><!--한글 설정-->
    
font-family: 'Song Myung', serif;    

```

- 컨트롤러에서 스트링으로 리턴하여 html 결과를 알고 싶을 때에는 html 자료를 templates에 넣어줘야 한다.

1. dto 만들기

   ```java
   private Timestamp indate;//타임 스탬프 dto
   ```

2. dao, service, serviceimpl 만들기

   - serviceImpl에 @Service @Primary 추가

3. 메인 화면 구현

   ```java
   <table border="1"> 
       <tr>
       	 <th>번호</th>
            <th>장소명</th>
            <th>주소</th>
            <th>이미지</th>
            <th>설명</th>
            <th>수정</th>
        </tr>
        <tr th:each="date_course:${date_course_list}">
       	 <td th:text="${date_course.sequence}">sequence</td>
   		 <td th:text="${date_course.place_name}">place_id</td>
   		 <td th:text="${date_course.place_address}">total_distance</td>
           <!--  <td th:text="${date_course.img}">total_distance</td> -->
            <td><img th:src="@{http://localhost:8080/images/}+${date_course.img}"></td>
            <!-- <td><a th:href="@{http://localhost:8080/}+${date_course.img}">이미지</td> -->
            <td th:text="${date_course.comment}">place_comment</td>
        	 <td><a th:href="@{/update_date_course(num=${date_course.num})}">수정</a></td> 
        </tr> 
   </table>
   ```

   ```java
   //테이블 행과 열 바꾸기
   
   tr { display: block; float: left; }
   	th, td { display: block; }
   ```

   - view-> 서버 get 방식으로 요청

   ```html
   <a href="/view/product?Heels">Heels</a>
   ```

   - 서버에서 받기

   ```java
   @GetMapping("/view")
   public String view_product
   ```

   - 타임리프 a태그
   
   ```html
   <a th:href="@{/update_date_course(num=${date_course.num})}">수정</a>
   ```
   
   - 로그인할 때와 로그아웃했을 때 보여지는 것 다르게
   
   ```html
   <a th:if="${log.equals('logout')}" href = "logout">로그아웃</a>
   <a th:unless="${log.equals('logout')}" href = "login">로그인</a>
   <a th:unless="${log.equals('logout')}" href = "member/join.html">회원가입</a>
   <a th:if="${log.equals('logout')}" href = "edit">회원정보 수정</a>
   <a th:if="${log.equals('logout')}" href = "map">랜덤 데이트 코스 검색
   ```
   
   - 타임리프 값 post방식으로 폼에서 보내기
   
   ```html
   <input type = "hidden" name = "pseq" th:value = "${product.pseq}">
   ```
   
   - 타임리프 객체 보내기
   
   ````html
   <form th:object="${}">
   ````
   
   - script 저장할 것
   
   ```html
   <script type="text/javascript" src="https://cdn.iamport.kr/js/iamport.payment-1.1.5.js"></script>
   <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
   <script src="/js/shopping.js" type="text/javascript"></script>
   ```
   
   

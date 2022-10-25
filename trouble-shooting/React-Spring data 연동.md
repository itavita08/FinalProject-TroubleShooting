## React-Spring data 연동
### 문제점
게시물 작성 후 게시물 제목 : `title`, 게시물 내용 : `content`, 이미지 이름 : `value` 데이터들이 Spring으로 넘어가지 않는 문제가 발생했었다.   
React, WEB 에서 아무런 에러가 발생하지 않았으며 Spring에서     
`org.springframework.web.bind.missingservletrequestparameterexception : Required request parameter 'title' for method parameter type String is not present`이 발생하였다.

### 해결방법
에러를 검색하였을때 컨트롤러의 코드상에서 파라미터를 요구하였는데 요청에 있어서 해당 필수 파라미터가 결여되어있기 때문에 발생하는 에러라고 한다.   
그래서 Spring에서 React가 보내주는 데이터를 못받는거라 가정하고 Spring에서 사용하는 어노테이션들을 찾아봤다.
찾아보던 중 `@RequestParam` 과 `@RequestBody`와의 차이점에 대해 알게되었다.   
`@RequestParam`은 url상에서 데이터를 찾고 `@RequestParam`은 json형태의 HTTP Body를 java객체로 변환을 시켜준다.   
React에서 Spring으로 데이터를 보낼때 data는 `json`형태로 넘어가게 되고 `@RequestParam`에서는 json형태인 data를 받지 못해서 생기는 에러였다.   
`@RequestParam`을 사용하니 데이터가 잘 넘어온다.   

### 문제 코드
```java
	@PostMapping("/test1")
	public String TestMethod(@RequestParam(value = "title") String title) { 
		System.out.println(title);
		return title;
	}
```

### 해결 코드
```java
	@PostMapping("/test1")
	public String TestMethod(@RequestBody String title) { 
		System.out.println(title);
		return title;
	}
```

 

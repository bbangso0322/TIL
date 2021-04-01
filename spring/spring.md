# Spring
> Spring makes programming Java quicker, easier, and safer for everybody. Spring’s focus on speed, simplicity, and productivity has made it the world's most popular Java framework.

Java 기반 웹 프레임워크 스프링  

스프링 부트를 사용하여 간단한 웹 프로젝트를 진행하면서 사용법을 익히도록 한다.  
<br/>
## 설치
스프링 공식 홈페이지 Spring initializer를 사용하여 프로젝트를 생성한다. 다음과 같은 설정을 하고 generate 한다.


![spring_initializer](./img/spring_initializer.PNG)  
<br/>
설치된 프로젝트를 열고 main class를 실행한 뒤 로컬호스트를 통해 접속했을 때 다음과 같은 화면이 뜨면 설치가 제대로 된 것이다.

![spring_main](./img/spring_main.PNG)  

## 기본 동작 방식
`static` 경로에 다음과 같은 `index.html` 파일을 추가하고 실행해보도록 한다.
``` html
<!-- src/main/resources/static/index.html-->
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>

<body>
Hello
<a href="/hello">hello</a>
</body>
</html>
```  

스프링부트는 `src/index.html` 파일을 welcome page로 사용한다.

다음은 contorller를 통해 get 요청을 처리해 보도록 한다.

다음과 같은 두 소스코드를 생성하고 브라우저를 통해 `localhost:8080/hello`에 접속해 본다.
```java
// src/main/java/hello/hellospring/controller/HelloController.java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HelloController {

    @GetMapping("hello")
    public String hello(Model model){
        model.addAttribute("data", "hello!!");
        return "hello";
    }
}
```  

```html
<!--src/resources/templates/hello.html-->
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```  

* `@GetMapping` annotation을 통해 get `/hello` 요청이 들어 왔을 경우 실행될 mathod를 매핑한다.   

* `model.addAttribute`를 통해 `attributeName`으로 data를, `attributeValue`로 "hello!!"라는 값을 바인딩한다.

* html 파일에서 `${data}` 를 통해 바인딩 된 값을 출력한다. 


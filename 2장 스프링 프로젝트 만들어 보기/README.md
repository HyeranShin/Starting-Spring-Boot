# 2장 스프링 프로젝트 만들어 보기
#### 스프링 부트의 주요 장점
- 프로젝트에 따라 자주 사용되는 라이브러리들이 미리 조합되어 있음
- 복잡한 설정을 자동으로 처리해줌
- 내장 서버를 포함해서 톰캣과 같은 서버를 추가로 설치하지 않아도 바로 개발 가능
- 톰캣이나 제티(Jetty)와 같은 웹 애플리케이션 서버(WAS; Web Application Server)에 배포하지 않고도 실행할 수 있는 JAR 파일로 웹 애플리케이션 개발 가능

## 2.2 Hello Wordl 만나 보기
HelloController.java
~~~java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController ❶
public class HelloController {

    @RequestMapping("/") ❷
    public String hello() { ❸
        return "Hello World!";
    }
}
~~~
❶ @RestController 어노테이션을 이용하여 해당 클래스가 REST 컨트롤러 기능을 수행하도로 함<br/>
❷ @RequestMapping 어노테이션은 해당 메서드를 실행할 수 있는 주소 설정. 여기서는 애플리케이션의 기본 주소를 /로 지정함. 이는 특별한 주소를 입력하지 않았을 때 실행되는 주소를 의미<br/>
❸ Hello World!라는 메시지를 화면에 전달해주는 역할. 바로 위에서 @RequestMapping 어노테이션에서 설정한 주소가 호출되면 해당 주소와 연관된 메서드 실행됨. 위에서는 기본 주소가 호출되면 hello 메서드가 실행되는 것을 알 수 있음<br/>

## 2.3 스프링 부트 프로젝트 살펴보기
|프로젝트의 주요 파일 및 구조|의미|
|-|-|
|src/main/java|자바 소스 디렉터리|
|SampleApplication|애플리케이션을 시작할 수 있는 main 메서드가 존재하는 스프링 구성 메인 클래스|
|templates|스프링 부트에서 사용 가능한 여러 가지 뷰 템플릿(Thymeleaf, Velocity, FreeMarker 등등) 파일 위치|
|static|스타일 시트, 자바스크립트, 이미지 등의 정적 리소스 디렉토리|
|application.properties|애플리케이션 및 스프링의 설정 등에서 사용할 여러 가지 프로퍼티(property) 정의|
|Project and External Dependencies|그레이들에 명시한 프로젝트의 필수 라이브러리 모음|
|src|JSP 등 리소스 디렉토리|
|build.gradle|그레이들 빌드 명세, 프로젝트에 필요한 라이브러리 관리, 빌드 및 배포 설정|

이 외에도 test 폴더에는 JUnit을 이용한 테스트 클래스가 있으며 gradle 폴더 및 gradlew, gradlew.bat은 그레이들 설정 및 실행 관련 스크립트 정보를 가지고 있음

### 2.3.1 SampleApplication 클래스
SampleApplication은 스프링 부트 애플리케이션의 구성과 실행을 담당하는 중요한 클래스<br/>
SampleApplication.java
~~~java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication ❶
public class SampleApplication {

    public static void main(String[] args) { ❷
        SpringApplication.run(SampleApplication.class, args);
    }
}
~~~
❶ @SpringBootApplication 어노테이션은 스프링 부트의 핵심 어노테이션이라해도 무방할 정도로 다양한 기능을 함. 이 어노테이션은 스프링 부트의 어노테이션 세 개로 구성되어 있음<br/>
@EnableAutoConfiguration<br/>
앞에서 스프링 부트의 장점 중 하나는 자동구성이라고 이야기 함. @EnableAutoConfiguration 어노테이션을 사용하기만 하면 스프링의 다양한 설정이 자동으로 완료<br/>
@ComponentScan<br/>
기존의 스프링은 빈(bean) 클래스를 사용하기 위해서 XML에 일일이 빈을 선언해줘야 했음. 그렇지만 @ComponentScan 어노테이션은 컴포넌트 검색 기능을 활성화해서 자동으로 여러 가지 컴포넌트 클래스를 검색하고 검색된 컴포넌트 및 빈 클래스를 스프링 애플리케이션 컨텍스트에 등록하는 역할을 함. 앞에서 "Hello World!"를 출력하기 위한 컨트롤러를 작성할 때 @RestController라는 어노테이션을 사용하여 스프링 MVC 컨트롤러를 만들었음. 이는 컴포넌트 자동 검색 및 구성에 의해서 그 컨트롤러를 사용할 수 있도록 한 것<br/>
@Configuration<br/>
@SpringBootApplication 어노테이션은 엄밀히 말을 해서 @Configuration이라는 어노테이션을 직접적으로 포함하 않음. 대신 @SpringBootConfiguration이라는 어노테이션이 포함되어 있는데, 이 어노테이션에 @Configuration이 포함되어 있음<br/>
@Configuration이 붙은 자바 클래스는 자바 기반 설정 파일임을 의미함. 스프링 3.x 버전까지는 자바 기반의 설정이 불가능하였음. 따라서 개발자들은 소위 XML 지옥이라고도 불리는 XML 기반의 설정에 오랜 시간을 투자해야 했음. 그렇지만 스프링 4.x 버전이 출시되면서 자바 기반의 설정이 가능하게 됨. 자바 기반의 설정은 @Configuration 어노테이션이 붙은 클래스 설정 파일임을 스프링 프레임워크에 알려줌. 앞으로 기능을 추가하면서 자 기반의 설정이 필요한 경우에는 항상 @Configuration 어노테이션을 사용할 것<br/>
❷ 앞에서 스프링 부트 프로젝트의 장점으로 톰캣이나 제티와 같은 웹 애플리케이션 서버에 배포하지 않고도 실행할 수 있는 JAR 파일로 웹 애플리케이션을 개발할 수 있다는 말을 함. main 메서드는 스프링 부트의 SampleApplication.run() 메서드를 사용하여 스프링 부트 애플리케이션을 실행할 수 있게 함. 스프링 부트 프로젝트를 생성하면서 단 하나의 XML 설정 파일도 사용하지 않았음. 심지어 웹 애플리케이션에는 꼭 있어야만 했던 web.xml도 사용하지 않았음. 이렇게 스프링 부트로 생성된 애플리케이션은 순수 자바만을 이용해서 개발을 할 수 있도록 함 (단 일부 라이브러리는 XML 기반의 설정이 필요할 수도 있음)<br/>


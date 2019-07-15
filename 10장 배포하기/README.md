# 10장 배포하기
## 10.1 스웨거를 이용한 REST API 문서화하기
### 10.1.1 스웨거란?
스웨거(Swagger)란 간단한 설정으로 프로젝트의 API 목록을 웹어서 확인 및 테스트를 할 수 있게 해주는 라이브러리<br/>
- 스웨거를 사용하면 컨트롤러에 정의되어 있는 모든 URL을 바로 확인할 수 있음
- API의 목록뿐 아니라 API의 명세 및 설정도 보여줌
- API를 직접 테스트해볼 수도 있음

### 10.1.2 스웨거 적용하기
build.gradle에 스웨거 라이브러리 추가
~~~java
compile group: 'io.springfox', name: 'springfox-swagger2', version: '2.9.2'
compile group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.9.2'
~~~
configuration 패키지 밑에 SwaggerConfiguration 클래스 생성
SwaggerConfiguration.java
~~~java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfiguration {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .select()
            .apis(RequestHandlerSelectors.basePackage("board")) ❶
            .paths(PathSelectors.any()) ❷
            .build();
    }
}
~~~
❶ board 패키지 내에 RequestMapping으로 할당된 모든 URL 선택<br/>
❷ PathSelectors.any("/api/**")와 같이 사용하면 특정 URI를 가진 주소만 선택할 수 있음. 여기서는 특정 경로는 선택하지 않고 모든 URL을 선택함<br/>

이제 애플리케이션을 실행시키고 localhost:8080/swagger-ui.html을 호출하면 스웨거 문서가 나타남<br/>
애플리케이션에 있는 모든 컨트롤러의 목록이 보임. 이는 앞에서 스웨거의 대상을 board 패키지의 모든 URL을 설정했기 때문임. 여기서 각 목록을 클릭하면 각 컨트롤러의 API 목록이 나옴. 목록 중 REST API 형식으로 개발된 rest-board-api-controller를 클릭하면 다음과 같이 RestBoardApiController의 모든 URL이 보임<br/>
HTTP 메서드, 호출 주소와 해당되는 메서드 이름까지 색깔별로 예쁘게 나옴. 여기서 각 목록을 클릭하면 좀 더 자세한 정보를 볼 수 있음. 게시글 상세화면 API인 GET /board/{boardIdx}를 클릭해서 어떤 정보르 확인할 수 있는지 살펴보겠음<br/>

해당 API에서 사용하는 파라미터와 파라미터의 설명, 응답 코드와 응답 코드에 따른 결과까지 확인할 수 있음<br/>

클라이언트 개발자는 스웨거의 문서만으로 API 호출에 필요한 파라미터와 응답 결과를 한눈에 알 수 있고, 백엔드 개발자는 귀찮은 문서를 작성하지 않아도 됨. 또한 해당 API를 호출해서 테스트를 할 수도 있음. 게시그 상세내용 조회 API를 예로 들어 살펴보겠음.<br/>

[Try it out] 버튼을 클릭하면 다음과 같이 API 호출에 필요한 파라미터를 입력하는 창이 나옴<br/>
여기서 API에 필요한 파라미터를 입력함. 해당 API에서 사용되는 파라미터가 나옴. API를 호출할 때 필요한 파라미터는 없을 수도 있고 한 개 또는 여러 개가 필요할 수도 있음. 파라미터가 여러 개일 경우 어떠한 파라미터가 필수인지 구분이 쉽지 않음. 그렇지만 스웨거는 필수 파라미터와 선택적 파라미터르 구분해서 보여줌. 게시글 내용을 조회하는 API에서 필수 파라미터는 게시글 번호를 의미하는 boardIdx임. boardIdx에 붙어있는 * required를 보고, 해당 파라미터가 필수라는 걸 확인 가능<br/>

[Execute]를 클릭하여 동작을 확인해보면 게시글 상세 내용 조회 API에 관련된 모든 정보가 나옴. Curl, 호출한 URL, 호출 결과 코드, 결과값, 결과값 헤더까지 API를 개발하고 사용하는 데 필요하 정보를 확인할 수 있음<br/>

### 10.1.3 스웨거에 설명 추가하기
RestBoardApiController.java
~~~java
@Api(description="게시판 REST API") ❶
@RestController
public class RestBoardApiController {
    @Autowired
    private BoardService boardService;
    
    @ApiOperation(value="게시글 목록 조회") ❷
    @RequestMapping(value="/api/board", method=RquestMethod.GET)
    ...중략...
    
    @ApiOperation(value="게시글 작성") ❸
    @RequestMapping(value="/api/board/write", method=RquestMethod.POST)
    ...중략...
    
    @ApiOperation(value="게시글 상세 내용 조회") ❹
    @RequestMapping(value="/api/board/{boardIx}", method=RquestMethod.GET)
    public BoardDto openBoardDetail(@PathVariable("boardIdx")
        @ApiParam(value="게시글 번호") int boardIdx) throws Exception { ❺
    ...중략...
    
    @ApiOperation(value="게시글 상세 내용 수정") ❻
    @RequestMapping(value="/api/board/{boardIx}", method=RquestMethod.PUT)
    ...중략...
    
    @ApiOperation(value="게시글 삭제") ❼
    @RequestMapping(value="/api/board{boardIx}", method=RquestMethod.DELETE)
    public String deleteBoard(@PathVariable("boardIdx")
        @ApiParam(value="게시글 번호") int boardIdx) throws Exception { ❽
    ...중략...
~~~
❶ @Api 어노테이션을 컨트롤러에 설명을 추가<br/>
❷❸❹❻❼ @ApiOperation 어노테이션으로 API에 설명 추가<br/>
❺❽ @ApiParam 어노테이션으로 API의 파라미터에 설명 추가<br/>

Board.java
~~~java
@ApiModel(value="BoardDto : 게시글 내용", description="게시글 내용") ❶
@Data
public class BoardDto {
    @ApiModelProperty(value="게시글 번호") ❷
    private int boardIdx;
    
    @ApiModelProperty(value="게시글 제목")
    private String title;
    
    @ApiModelProperty(value="게시글 내용")
    private String contents;
    
    @ApiModelProperty(value="조회수")
    private int hitCnt;
    
    @ApiModelProperty(value="작성자 아이디")
    private String creatorId;
    
    @ApiModelProperty(value="작성시간")
    private String createdDatetime;
    
    @ApiModelProperty(value="수정자 아이디")
    private String updaterId;
    
    @ApiModelProperty(value="수정시간")
    private String updatedDatetime;
    
    @ApiModelProperty(value="첨부파일 목록")
    private List<BoardFileDto> fileList;
}
~~~
❶ @ApiModel 어노테이션으로 모델에 설명 추가<br/>
❷ @ApiModelProperty 어노테이션으로 모델의 요소에 설명 추가<br/>

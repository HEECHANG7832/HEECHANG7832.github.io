



# 따라하기2



### Spring web 계증구조를 요약하자면



**Controller** "Service야 전달받은 Dto객체 저장좀 해줘"

- Controller 에서 Service 객체 생성, @PostMapping("/api/v1/posts")으로 Post 요청 연결
- @RequestBody PostsSaveRequestDto requestDto를 통해 post내용 전달받음

- PostsService postsService;

- postsService.save(requestDto); //전달받은 Dto를 Service객체를 통해 저장
- save, delete, update, findbyid 구현 전달되는 객체는 각각의 Dto



**Service** "Repository야 전달받은 Dto의 entity DB에 저장해줘"

- Repository(domain영역이자, DB접근)객체 생성, 전달받은 Dto
- Repository.save(requestDto.toEntity())명령 진행
- save, delete, update, findbyid 구현 Repository에 원하는 동작을 구동
- update에서는 왜 쿼리를 날리지 않을까?
  - JPA영속성 컨텍스트
  - Entity가 영속성 컨텍스트가 유지된 상태라면 트랜잭션이 끝나는 시점에 해당 테이블에 변경분을 반영 즉, Entity 객체의 값만 변경하면 별도로 Update를 날릴 필요가 없다



**Dto**

- Entity클래스와 거의 유사한 형태,  toEntity()를 구현(Builder패턴 사용)

- Controller에서 여러 테이블로 결과를 조인해서 전달해야 하는 경우가 많기 때문에 Entity를 직접 사용하는 것보다 표현력? 이 좋아진다

- 각 요청별 필요한 데이터를 가지고 있다 title, content등

  





객체가 단순히 데이터 덩어리가 되지 않게 하기 위해서 도메인의 객체가 각 객체에 정해진 동작을 수행 - 도메인 모델

@WebMvcTest는 JPA기능이 작동하지 않기 때문에 JPA기능까지 확인하려면 @SpringBootTest와 TestRestTemplate를 사용해야한다



### 생성시간/수정시간 자동화

- Entity에는 기본적으로 생성시간과 수정시간을 포함 - 유지보수에 필요

```java
import lombok.Getter;
import javax.persistence.EntityListeners;
import javax.persistence.MappedSuperclass;
import java.time.LocalDateTime;

//모든 Entity들의 상위 클래스가 되어 Entity들의 createdDate, modifiedDate를 자동으로 관리
@Getter
@MappedSuperclass //JPA Entity클래스들이 BaseTimeEntity를 상속할 경우 createdDate, modifiedDate도 컬럼으로 인식하게
@EntityListeners(AuditingEntityListener.class) //Auditing 기능 포함
public abstract class BaseTimeEntity {

    @CreatedDate //Entity가 생성될때 시간 자동 저장
    private LocalDateTime createdDate;

    @LastModifiedDate //조회한 Entity의 값을 변경할때 시간 자동 저장
    private LocalDateTime modifiedDate;

}

```

   

**Application.java**

```java
@EnableJpaAuditing
```



### 머스테치로 화면 구성

템플릿 엔진 - 저장된 템플릿 양식과 데이터를 합쳐 HTML 문서를 출력하는 소프트웨어

- JSP, Freemarker(서버 템플릿 엔진), React, View.js (클라이언트 템플릿 엔진)등
- 서버 템플릿 엔진은 서버에서 Java 코드가 실행되어 만들어진 결과를 전달
- 클라이언트 템플릿 엔진은 브라우저 위에서 작동, 서버는 JSON만 전달



**머스테치**

- 현존하는 대부분의 언어 지원, 자바에서는 서버 템플릿 엔진으로, 자바스크립트에서는 클라이언트 템플릿 엔진으로 사용
- 인텔리제이 커뮤니티 버전에서도 지원

**Thymeleaf**

	- 문법이 어렵다
	- HTML 테그에 속성으로 템플릿 기능을 사용하는 방식



**머스테치 적용**

- build.gradle에 의존성 등록
- src/main/resources/templates 에 머스테치 파일을 두면 스프링 부트가 자동으로 로딩
- intex.mustache 작성

**프론트엔드 외부 라이브러리 사용**

- 외부 CDN or 직접 라이브러리 받아서 사용

- 레이아웃 방식 - 공통역역을 별도의 파일로 분리해서 필요한 곳에서 가져가는 방식

- 이 영역에 제이쿼리와 부트스트랩을 로딩하는 코드를 추가한다

- footer.mustache

- header.mustache

   

**intex.mustache**

```html
{{>layout/header}}

    <h1>스프링부트로 시작하는 웹 서비스 Ver.2</h1>
    <div class="col-md-12">
        <div class="row">
            <div class="col-md-6">
                <a href="/posts/save" role="button" class="btn btn-primary">글 등록</a>
                {{#userName}}
                    Logged in as: <span id="user">{{userName}}</span>
                    <a href="/logout" class="btn btn-info active" role="button">Logout</a>
                {{/userName}}
                {{^userName}}
                    <a href="/oauth2/authorization/google" class="btn btn-success active" role="button">Google Login</a>
                    <a href="/oauth2/authorization/naver" class="btn btn-secondary active" role="button">Naver Login</a>
                {{/userName}}
            </div>
        </div>
        <br>
        <!-- 목록 출력 영역 -->
        <table class="table table-horizontal table-bordered">
            <thead class="thead-strong">
            <tr>
                <th>게시글번호</th>
                <th>제목</th>
                <th>작성자</th>
                <th>최종수정일</th>
            </tr>
            </thead>
            <tbody id="tbody">
            {{#posts}} //posts라는 List 순회
                <tr>
                    <td>{{id}}</td> //List의 각 객체의 필드 사용
                    <td><a href="/posts/update/{{id}}">{{title}}</a></td>
                    <td>{{author}}</td>
                    <td>{{modifiedDate}}</td>
                </tr>
            {{/posts}} 
            </tbody>
        </table>
    </div>

{{>layout/footer}}

```

   

개시글 등록을 위한 **index.js** 

- src/main/resources/static 에 위치한 자바스크립트, CSS, 이미지 등 정적파일은 URL에서 '/' 로 설정됨

```js
var main = {  //브라우저의 스코프는 공용공간이기 때문에 같은 이름의 function이 있을 경우 나중에 로딩된 js가 덮어쓰는것을 방지
    init : function () {
        var _this = this;
        $('#btn-save').on('click', function () {
            _this.save();
        });

        $('#btn-update').on('click', function () {
            _this.update();
        });

        $('#btn-delete').on('click', function () {
            _this.delete();
        });
    },
    save : function () {
        var data = {
            title: $('#title').val(),
            author: $('#author').val(),
            content: $('#content').val()
        };

        $.ajax({
            type: 'POST',
            url: '/api/v1/posts',
            dataType: 'json',
            contentType:'application/json; charset=utf-8',
            data: JSON.stringify(data)
        }).done(function() {
            alert('글이 등록되었습니다.');
            window.location.href = '/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    },
    update : function () {
        var data = {
            title: $('#title').val(),
            content: $('#content').val()
        };

        var id = $('#id').val();

        $.ajax({
            type: 'PUT',
            url: '/api/v1/posts/'+id,
            dataType: 'json',
            contentType:'application/json; charset=utf-8',
            data: JSON.stringify(data)
        }).done(function() {
            alert('글이 수정되었습니다.');
            window.location.href = '/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    },
    delete : function () {
        var id = $('#id').val();

        $.ajax({
            type: 'DELETE',
            url: '/api/v1/posts/'+id,
            dataType: 'json',
            contentType:'application/json; charset=utf-8'
        }).done(function() {
            alert('글이 삭제되었습니다.');
            window.location.href = '/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    }

};

main.init();


```



   

url 적용을 위한 **IntexController.java**

```java
@RequiredArgsConstructor
@Controller
public class IndexController {

    private final PostsService postsService;

    @GetMapping("/")
    public String index(Model model, @LoginUser SessionUser user) {
        //Model - 서버 템플릿 엔진에서 사용할 수 있는 객체를 가져올 수 있다.
        //postsService의 결과를 model(mustache)로 전달한다
        model.addAttribute("posts", postsService.findAllDesc());
        if (user != null) {
            model.addAttribute("userName", user.getName());
        }
        return "index";
    }

    @GetMapping("/posts/save")
    public String postsSave() {
        return "posts-save";
    }

    @GetMapping("/posts/update/{id}")
    public String postsUpdate(@PathVariable Long id, Model model) {
        PostsResponseDto dto = postsService.findById(id);
        model.addAttribute("post", dto);

        return "posts-update";
    }
}


```

  
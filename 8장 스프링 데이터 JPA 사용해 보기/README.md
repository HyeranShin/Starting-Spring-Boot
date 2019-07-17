# 8장 스프링 데이터 JPA 사용해 보기
## 8.1 스프링 데이터 JPA
스프링 데이터 JPA는 JPA를 스프링에서 쉽게 사용할 수 있도록 도와주는 프레임워크로 내부적으로 하이버네이트를 이용해서 기능을 구현하고 있음

### 8.1.1 JPA란?
JPA(Java Persistence API)란 자바 객체와 데이터베이스 테이블 간의 매핑을 처리하는 ORM(Obejct Relational Mapping) 기술의 표준임. ORM을 단순하게 이야기하면 객체와 관계를 설정하는 것임. 여기서 객체란 우리가 흔히 알고 있는 OOP의 객체이고, 관계란 개발자들이 사용하는 관계형 데이터베이스를 의미함. 그렇다면 무엇 때문에 객체와 관계형 데이터베이스의 매핑을 도와주는 ORM 기술들이 발전할까?<br/>
ORM은 객체지향 프로그래밍에서 사용하는 객체와 데이터베이스의 테이블의 구조가 비슷하다는 점에서 시작함. ORM은 특정한 언어에 종속적인 개념이 아니라 객체와 관계형 데이터베이스(RDBMS)를 매핑시킨다는 개념임. 이러한 ORM의 개념을 구현하기 위한 표준이 JPA임<br/>
JPA는 각 기능의 동작이 어떻게 되어야 한다는 것을 정의한 기술 명세이기 때문에 기술 명세에 따라 실제로 기능을 구현한 구현체가 필요함. 이러한 구현체로 JPA의 표준이 정의하 기능을 구현하는게 필요하고, 이를 구현한 제품이나 프레임워크로 하이버네이트, 이클립스링크(EclipseLink) 등이 있음. 이러한 JPA 구현체를 JPA 프로바이더라고 함<br/>
각각의 JPA 프로바이더는 JPA 표준이 정의한 기능을 구현하기는 하지만 표준이 정의하지 않은 기능의 경우 JPA 프로바이더마다 서로 다르게 구현됨. JPA 프로바이더 중 가장 많이 사용되는 것은 하이버네이트로, 인터넷에서 JPA를 검색하면 대부분 하이버네이트에 대한 내용이 나올 정도임

### 8.1.2 JPA의 장점
1. 개발이 편리함<br/>
웹 애플리케이션에서 반복적으로 작성하는 기본적인 CRUD(Create, Read, Update, Delete)용 SQL을 직접 작성하지 않아도 됨
2. 데이터베이스 독립적인 개발이 가능<br/>
JPA는 특정 데이터베이스에 종속적이지 않기 때문에 데이터베이스와 관계 없이 개발할 수 있음. 데이터베이스가 변경되더라도 JPA가 해당 데이터베이스에 맞는 쿼리를 생성해줌
3. 유지보수가 쉬움<br/>
마이바티스와 같은 매퍼 프레임워크를 사용해서 데이터베이스 중심의 개발을 하며 테이블이 변경될 경우 관련된 코드를 모두 변경해야 함. 그렇지만 JPA를 사용하면 객체(JPA의 엔티티)만 수정하면 됨

### 8.1.3 JPA의 단점
1. 학습곡선(learning curve)이 큼<br/>
기존의 데이터베이스 위주의 개발방식에 비해서 배워야 할 것들이 많음. 또한 SQL을 직접적으로 작성하지 않기 때문에 튜닝 등을 할 때도 어려움을 겪음
2. 특정 데이터베이스의 기능을 사용할 수 없음<br/>
JPA를 사용할 때 특정 데이터베이스에 종속적인 기능은 쓰지 않는 것이 좋음. 특히 오라클은 강력한 함수를 많이 제공하는데, 이와 같은 데이터베이스에 종속적인 기능을 사용할 경우 데이터베이스에 독립적인 개발이 불가능해짐. 이 경우 데이터베이스에 독립적인 개발이 가능한 JPA의 장점을 잃게 됨
3. 객체지향 설계가 필요함<br/>
그렇지만 객체 위주의 설계보다 데이터베이스의 테이블을 설계하고 그에 맞춰서 객체 및 비즈니스 로직이 설계, 개발되기 때문에 객체지향적인 설계가 어려울 수 있음

### 8.1.4 스프링 데이터 JPA란?
스프링 데이터 JPA란 JPA를 스프링에서 쉽게 사용할 수 있도록 해주는 라이브러리임. 하이버네이트와 같은 JPA 프로바이더를 직접 사용할 경우 엔티티 매니저(EntityManager)를 설정하고 이용하는 등 여러 가지 진입장벽이 있음. 스프링 데이터 JPA는 리포지터리(Repository)라는 인터페이스를 제공함. 이 인터페이스만 상속받아 정해진 규칙에 맞게 메서드를 작성하면 개발자가 작성해야 할 코드가 완성됨. 그리고 내부적으로는 실제 기능을 담당하는 JPA의 구현체, 다른 말로 JPA 프로바이더로 하이버네이트를 사용함. 하이버네이트를 모르더라도 프레임워크가 하이버네이트를 이용해서 적절한 코드를 생성하기 때문에 JPA를 쉽게 사용할 수 있음

## 8.3 JPA를 사용한 게시판으로 변경하기
### 8.3.1 엔티티 생성하기
BoardEntity.java
~~~java
import java.time.LocalDateTime;
import java.util.Collection;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.OneToMany;
import javax.persistence.Table

import lombok.Data;
import lombok.NoArgsConstructor;

@Entity ❶
@Table(name="t_jpa_board") ❷
@NoArgsConstructor
@Data
public class JpaBoard {
    @Id ❸
    @GeneratedValue(strategy=GenerationType.AUTO) ❹
    private int boardIdx;
    
    @Column(nullable=false) ❺
    private String title;
    
    @Column(nullable=false)
    private String contents;
    
    @Column(nullable=false)
    private int hitCnt = 0;
    
    @Column(nullable=false)
    private String creatorId;
    
    @Column(nullable=false)
    private LocalDateTime createdDatetime = LocalDateTime.now(); ❻
    
    private String updaterId;
    
    private LocalDateTime updatedDatetime;
    
    @OneToMany(fetch=FetcyType.EAGER, cascade=CascadeType.ALL) ❼
    @JoinColumn(name="board_idx")
    private Collection<BoardFileEntity> fileList;
}
~~~
❶ @Entity 어노테이션은 해당 클래스가 JPA의 엔티티임을 나타냄. 엔티ㅣ 클래스는 테이블과 매핑됨<br/>
❷ t_jpa_board 테이블과 매핑되도록 나타냄<br/>
❸ 엔티티의 기본키(Primary Key, PK)임을 나타냄<br/>
❹ 기본키의 생성 전략을 설정함. GenerationType.AUTO로 지정할 경우 데이터베이스에서 제공하는 기본키 생성 전략을 따르게 됨. MySQL은 자동 증가를 지원하므로 기본키가 자동으로 증가하며, 자동 증가가 지원되지 않는 오라클의 경우 기본키에 사용할 시퀀스를 생성하게 됨<br/>
❺ 컬럼에 Not Null 속성을 지정함<br/>
❻ 작성시간의 초기값을 설정함. @Column 어노테이션을 이용해서 초기값을 지정할 수도 있지만 사용하는 데이터베이스에 따라서 초기값을 다르게 설정해야 할 수도 있음. 하지만 이렇게 하면 데이터베이스의 종류와 관계없이 사용할 수 있는 JPA의 장점이 퇴색되기 때문에 데이터베이스에 의존적인 초기값은 사용하지 않는 것이 좋음<br/>
❼ @OneToMany는 1:N의 관계를 표현하는 JPA 어노테이션임. 하나의 게시글은 기본적으로 첨부파일이 없거나 1개 이상의 첨부파일을 가질 수 있음. 이 경우 게시글 하나에 여러 개의 첨부파일을 가지는 1:N 관계가 됨. @JoinColumn 어노테이션은 릴레이션 관계가 있는 테이블의 컬럼을 <br/>

### 8.3.5 리포지터리 작성하기
리포지터리(Repository)는 스프링 데이터 JPA가 제공하는 인터페이스임. 스프링 데이터 JPA가 제공하는 리포지터리 인터페이스는 몇 가지 종류가 있음. 여기서는 가장 간단히 사용할 수 있는 CrudRepository 인터페이스를 사용해봄<br/>
JpaBoardRepository.java
~~~java
import java.util.List;

import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.CrudRepository;
import org.springframework.data.repository.query.Param;

import board.board.entity.JpaBoard;
import board.board.entity.JpaBoardFile;

public interface JpaBoardRepository extends CrudRepository<BoardEntity, Integer> { ❶
    List<BoardEntity> findAllByOrderByBoardIdxDesc(); ❷
    
    @Query("SELECT file FROM BoardFileEntity file WHERE board_idx = :boardIdx AND idx = :idx") ❸
    BoardFileEntity findBoardFile(@Param("boardIdx") int boardIdx, @Param("idx") int idx);
}
~~~
❶ 스프링 데이터 JPA에서 제공하는 CrudRepository 인터페이스를 상속받음. CrudRepository 인터페이스는 리포지터리에서 사용할 도메인 클래스와 도메인의 id 타입을 파라미터로 받음<br/>
❷ 게시글 번호로 정렬해서 전체 게시글을 조회함. 규칙에 맞도록 리포지터리에 메서드를 추가하면 실행 시 메서드의 이름에 따라 쿼리가 생성되어 실행됨<br/>
❸ @Query 어노테이션을 이용해서 첨부파일의 정보를 조회함. @Query 어노테이션을 사용하면 실행하고 싶은 쿼리를 직접 정의할 수 있음. 쿼리 메서드로도 개발할 수 있지만 @Query 어노테이션의 사용법을 보기 위해 사용함 <br/>

JpaBoardServiceImpl 클래스에서 여러 개의 리포지터리 메서드를 호출했는데, JpaBoardRepository 인터페이스에는 단 두 개의 메서드만 정의되어 있음. CrudRepository 인터페이스에서 제공하는 기능을 이용했기 때문에 앞의 Mapper 인터페이스에서 작성했던 것과 비교해서 많은 수의 메서드를 정의하지 않아도 됨<br/>

<b>스프링 데이터 JPA 리포지터리 인터페이스</b><br/>
최상위의 Repository 인터페이스에는 아무런 기능이 없기 때문에 잘 사용하지 않음. CrudRepository 인터페이스는 이름에서 알 수 있듯이 CRUD(Create, Read, Update, Delete) 기능을 기본적으로 제공함. 따라서 CrudRepository 인터페이스를 이용할 경우 CRUD에 해당하는 기능을 작성하지 않더라도 인터페이스에서 제공되는 기능을 바로 사용할 수 있음<br/>
PagingAndSortingRepository 인터페이스는 CrudRepository 인터페이스의 기능에 페이징 및 정렬 기능이 추가된 인터페이스임. JpaRepository 인터페이스는 PagingAndSortingRepository 인터페이스의 기능뿐만 아니라 JPA에 특화된 기능까지 추가된 인터페이스임<br/>
스프링 데이터 JPA를 이용하면 리포지터리 인터페이스를 기준으로 동적으로 실행할 수 있는 메서드를 생성해줌<br/>

<b>CrudRepository 인터페이스가 기본적으로 제공하는 메서드의 목록</b>

|메서드|설명|
|-|-|
|<S extends T> S save(S entity)|주어진 엔티티를 저장|
|<S extends T> Iterable<S> saveAll(Iterable<S> entities)|주어진 엔티티 목록을 저장|
|Optional<T> findById(ID id)|주어진 아이디로 식별된 엔티티를 반환|
|boolean existsById(ID id)|주어진 아이디로 식별된 엔티티가 존재하는지를 반환|
|Iterable<T> findAll()|모든 엔티티를 반환|
|Iterable<T> findAllById(Iterable<ID> ids)|주어진 아이디 목록에 맞는 모든 엔티티 목록을 반환|
|long count()|사용 가능한 엔티티의 개수를 반환|
|void deleteById(ID id)|주어진 아이디로 식별된 엔티티를 삭제|
|void delete(T entity)|주어진 엔티티를 삭제|
|void deleteAll(Iterable<? extends T> entities)|주어진 엔티티 목록으로 식별된 엔티티를 모두 삭제|
|void deleteAll|모든 엔티티 삭제|

<b>쿼리 메서드</b><br/>
스프링 데이터 JPA는 규칙에 맞게 메서드를 추가하면 그 메서드의 이름으로 쿼리를 생성하는 기능을 제공함. 이를 쿼리 메서드라고 함. 쿼리 메서드는 find...By, read...By, query...By, count...By, get...By로 시작해야 함. 첫 번째 By 뒤쪽은 컬럼 이름으로 구성됨. 즉, 첫 번째 By는 쿼리의 검색조건이 됨. 예를 들어서 앞에서 사용한 BoardEntry의 제목으로 검색하려면 다음과 같이 작성함<br/>

findByTitle(String title);<br/>

이 메서드가 실행되면 다음과 같은 JPQL(Java Persistence Query Language)문이 실행됨<br/>

select jpaboard0_.board_idx as board_id1_0_, ...중략...<br/>
from t_jpa_board jpaboard0_ where jpaboard0_.title=?<br/>

두 개 이상의 속성을 조합하려면 And 키워드를 사용하면 됨. 제목과 내용으로 검색하려면 다음과 같음<br/>

findByTitleAndContents(String title, String contents);<br/>

스프링 JPA는 이 외에 여러 가지 비교연산자를 지원함. 비교 연산자를 조합해서 필요한 쿼리 메서드를 작성함 <br/>

<b>스프링 데이터 JPA 레퍼런스 문서에 있는 비교연산자의 목록</b>

|키워드|예시|JPQL 변환|
|-|-|-|
|And|findByLastnameAndFirstname|... where x.lastname = ?1 and x.firstname = ?2|
|Or|findByLastnameOrFirstname|... where x.lastname = ?1 or x.firstname = ?2|
|Is, Equals|findByFistname<br/>findByFistnameIs<br/>findByFirstnameEquals|... where x.firstname = ?1|
|Between|findByStartDateBetween|... where x.startDate between ?1 and ?2|
|LessThan|findByAgeLessThan|... where x.age < ?1|
|LessThanEqual|findByAgeLessThanEqual|... where x.age <= ?1|
|GreaterThan|findByAgeGreaterThan|... where x.age > ?1|
|GreaterThanEqual|findByAgeGreaterThanEqual|... where x.age >= ?1|
|After|findByStartDateAfter|... where x.startDate > ?1|
|Before|findByStartDateBefore|... where x.startDate < ?1|
|IsNull|findByAgeIsNull|... where x.age is null|
|isNotNull, NotNull|findByAge(Is)NotNull|... where x.age not null|
|Like|findByFirstnameLike|... where x.firstname like ?1|
|NotLike|findByFirstnameNotLike|... where x.firstname not like ?1|
|StartingWith|findByFirstnameStartingWith|... where x.firstname like ?1(파라미터 앞에 % 추가)|
|EndingWith|findByFirstnameEndingWith|... where x.firstname like ?1(파라미터 앞에 % 추가|
|Containing|findByFirstnameContaining|... where x.firstname like ?1(파라미터 앞, 뒤로 % 추가|
|OrderBy|findByAgeOrderByLastnameDesc|... where x.age = ?1 order by x.lastname desc|
|Not|findByLastnameNot|... where x.lastname <> ?1|
|In|findByAgeIn(Collection<Age> ages)|... where x.age in ?1|
|NotIn|findByAgeNotIn(Collection<Age> ages|... where x.age not in ?1|
|TRUE|findByActiveTrue()|... where x.active = true|
|FALSE|findByActiveFalse()|... where x.active = false|
|IgnoreCase|findByFirstnameIgnoreCase|... where UPPER(x.firstname) = UPPER(?1)|

<b>@Query 사용하기</b><br/>
메서드 이름이 복잡하거나 쿼리 메서드로 표현하기 힘들다면 @Query 어노테이션으로 쿼리를 직접 작성할 수 있음. 이때 쿼리는 JPQL이나, 데이터베이스에 맞는 SQL을 이용할 수 있음. 쿼리를 작성할 메서드에 @Query 어노테이션을 적용하고 쿼리를 작성하면 됨<br/>

@Query 어노테이션을 사용한 JPQL
~~~java
@Query("SELECT file FROM BoardFileEntity file WHERE board_idx = ?1 AND idx = ?2")
BoardFileEntity findBoardFile(int boardIdx, int idx); ❶

@Query("SELECT file FROM BoardFileEntity file WHERE board_idx = :boardIdx AND idx = :idx")
BoardFileEntity findBoardFile(@Param("boardIdx") int boardIdx, @Param("idx") int idx); ❷
~~~
❶ [?숫자] 형식으로 파라미터를 지정함. 메서드의 파라미터 순서대로 각각 ?1, ?2에 해당됨. 즉, boardIdx는 ?1에 할당되고 idx는 ?2에 할당됨. 변수의 개수가 증가하면 숫자도 그만큼 증가함<br/>
❷ :[변수이름]으로 파라미터를 지정함. 변수이름은 메서드의 @param 어노테이션에 대응됨. :boardIdx의 boardIdx 변수는 @Param("boardIdx") 어노테이션이 있는 메서드의 파라미터를 사용함<br/>

일반적으로 두 번째 방식이 사용됨. 파라미터가 한두개만 존재하거나 쿼리가 간단할 경우에는 첫번째 방식을 사용하더라도 큰 문제가 없음. 하지만 파라미터의 개수가 많아지거나 쿼리의 길이가 길어질 경우에는 쿼리 파라미터와 메서드 파라미터를 쉽게 알아볼 수 없고 파라미터의 순서를 바꿔 입력하는 등의 예상치 못한 오류를 만들기도 쉬움. 따라서 두번째 방식을 이용해서 개발하는 것이 좋음<br/>

JPQL을 작성할 때 유념해야 할 사항은 쿼리의 FROM 절에 데이터베이스의 테이블 이름이 아니라 검색하려는 엔티티의 이름을 사용한다는 것

---
layout: post
title:  "spring boot 스터디 2주 차"
date:   2020-01-30 22:13:00 +0900
categories: springboot intellij
---

## 1. Entity 설정
`main > java > 프로젝트명 > domain` 폴더에 Board.java 파일 생성
```java
@Entity
@Builder
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Board {
    @Id
    @GeneratedValue
    private Long id;

    private String author;
    private String subject;
    private String content;
    private LocalDateTime createdTime;
}
```
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-30_002.png)
- @Entity : Spring container에 Entity Bean이라고 선언.
Bean을 선언하지 않으면 단순 Java 파일이며 Spring에서 관리하지 않는다.

- @Builder : Lombok에서 지원하는 Annotation이다.
객체를 생성할때 사용된다.

- @Getter @Setter : Lombok에서 지원하는 자동 코드 생성 Annotation으로
클래스의 맴버 변수를 접근할 수 있는 Getter와 설정할 수 있는 Setter 메소드를 생성해 준다.

- @NoArgsConstructor : 매개변수 없는 생성자를 만들어 준다.
- @AllArgsConstructor : 모든 맴버변수를 매개변수로 정의된 생성자를 만들어 준다.


## 2. JPA Repository 생성
`main > java > 프로젝트명 > repository` 폴더에 BoardRepository.java 파일 생성
```java
@Repository
public interface BoardRepository extends JpaRepository<Board, Long> {
}
```
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-30_003.png)
- @Repository : Spring의 Bean으로 DAO역할을 하는 Bean을 선언한 것.
- `<Board, Long>` : JpaRepository를 상속할때 2개의 타입을 제네릭으로 정의해야 한다.
첫 번째(Board) 타입은 Repository에서 사용될 Entity 타입이며,
두 번째(Long) 타입은 Board Entity의 @ID(Primary key)의 데이터 타입이다.
즉, Board에 @Id가 Long 타입이기 때문이다.

## 3. Test Case 생성
BoardRepository.java 파일에서 단축키 `Ctrl + Shift + T`를 눌러 Create New Test 선택.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-30_001.png)

OK버튼을 눌러 JUnit4 테스트 케이스를 생성한다.

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class BoardRepositoryTest {

    @Autowired
    BoardRepository boardRepository;

    @Test
    public void Board등록정상테스트() {
        //given
        Board requestData = Board.builder()
                .author("박규태")
                .content("내용")
                .createdTime(LocalDateTime.now())
                .subject("제목")
                .build();

        //when
        Board newBoard = boardRepository.save(requestData);

        //then
        assertEquals("박규태", newBoard.getAuthor());
        assertEquals("내용", newBoard.getContent());
        assertEquals("제목", newBoard.getSubject());
        assertNotNull(newBoard.getId());
        assertNotNull(newBoard.getCreatedTime());
    }

    @Test
    public void Board수정테스트() {
        //given
        Board requestData = Board.builder()
                .author("박규태")
                .content("내용")
                .createdTime(LocalDateTime.now())
                .subject("제목")
                .build();

        Board newBoard = boardRepository.save(requestData);

        //when
        newBoard.setAuthor("홍석춘");
        Board selectBoard = boardRepository.findById(newBoard.getId()).get();

        //then
        assertEquals("홍석춘", selectBoard.getAuthor());
        assertEquals(requestData.getAuthor(), selectBoard.getAuthor());
        assertEquals(newBoard.getId(), selectBoard.getId());
        assertEquals(requestData, newBoard);
        assertEquals(selectBoard, newBoard);
    }
}
```
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-30_004.png)

끄읏.
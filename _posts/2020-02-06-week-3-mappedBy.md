---
layout: post
title:  "spring boot 스터디 3주 차"
date:   2020-02-06 12:48:00 +0900
categories: springboot intellij mappedBy
---

## 1. Board Entity 수정
`main > java > 프로젝트명 > domain` 폴더에 Board.java 파일 수정
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
    @Column(name = "board_id")
    private Long id;

    private String author;
    private String subject;
    private String content;
    private LocalDateTime createdTime;
    private LocalDateTime modifiedDate;
    private int hitCount;
    private boolean delYn;
    private String password;

    @Builder.Default
    @OneToMany(mappedBy = "board")
    private List<Comments> commentList = new ArrayList<>();

    /*
    게시글 등록
     */
    public static Board postBoard(String author, String subject, String content, String password) {
        return Board.builder()
                .author(author)
                .subject(subject)
                .content(content)
                .hitCount(0)
                .delYn(false)
                .password(password)
                .createdTime(LocalDateTime.now())
                .modifiedDate(LocalDateTime.now())
                .build();
    }

    /*
    게시글 조회
     */
    public int readBoard() {
        return this.hitCount++;
    }

    /*
    게시글 수정
     */
    public void adjustBoard(String subject, String content) {
        this.subject = subject;
        this.content = content;
        this.modifiedDate = LocalDateTime.now();
    }

    /*
    게시글 삭제
     */
    public void delete() {
        this.delYn = true;
    }

}
```

## 2. Comments Entity 생성
`main > java > 프로젝트명 > domain` 폴더에 Comments.java 파일 생성
```java
@Entity
public class Comments {
    @Id
    @GeneratedValue
    @Column(name = "comments_id")
    private Long id;
    private String content;

    @ManyToOne
    @JoinColumn(name = "board_id")
    private Board board;
}
```

#### Entity 관계 설정 Annotation
- @OneToOne : 1:1 관계
- @ManyToOne : N:1 관계
- @OneToMany : 1:N 관계
- @ManyToMany : N:N 관계(실무적으로도 잘 사용하지 않으며, 가능하면 사용하지 않도록 한다.)

#### 연관관계의 주인
잘 알고 있겠지만 데이터베이스에서는 테이블의 관계를 설정할때 하위 개념의 테이블에 Forgine Key(FK)를 추가한다. 즉, 일반적인 `1:N` 관계일때 `N`쪽의 테이블에 FK를 추가한다.

일반적으로 `1:N`에서 `1`쪽의 테이블을 상위 테이블 또는 주 테이블이라 하겠지만, JPA에서는 FK를 가진 테이블, `N쪽`의 테이블을 연관관계의 주인이라고 한다. 즉, 원장과 거래내역에서 거래내역이 연관관계의 주인이며, 학교와 학생에서는 학생이 연관관계의 주인이 된다. 여기서는 Comments가 연관관계의 주인 테이블이 된다.

#### 패러다임 불일치
데이터베이스의 관점에서 본다면 두 테이블의 관계 설정은 연관관계 주인 테이블(Comments)에 foreign key를 설정하면 된다.
```
create table Board (
    board_id bigint  # primary key
    ...
)

create table Comments (
    comment_id bigint # primary key
   , board_id bigint # foreign key
   ...
)
```
하지만 객체지향적인 관점에서는 두 Entity 모두 맴버변수로 연관관계를 가져야 한다.
```java
public class Board {
    @Id
    private Long board_id;

    // OOP 관점의 foreign key
    // 하지만 데이터베이스 관점에서는 존재하지 않는 컬럼이다.
    private List<Comments> commentList = new ArrayList<>();
    ...
}

public class Comments {
    @Id
    private Long comment_id;

    // OOP 관점의 foreign key
    private Board board;
    ...
}

```
 부모(Board) 객체에서 자식(Comments) 객체를 접근할 수 있어야 하고, 자식에서 부모로도 접근이 가능해야 하기 때문이다.
 이러한 DB와 OOP의 다른 상황을 `패러다임의 불일치`라고 한다.

#### mappedBy를 이용한 패러다임의 불일치 해결
 Board와 Comments 둘 다 @OneToMany, @ManyToOne으로 연관관계를 정의하게 되면 DB관점에서 Board에는 Comments FK가 만들어지고, Comments에는 Board FK가 만들어지게 된다.

DB관점에서 Board에는 FK가 필요없기 때문에 다음과 같이 mappedBy를 이용하여 Comments의 연관관계를 참조만하도록 만든다.
```java
public class Board {
    @Id
    private Long board_id;

    @OneToMany(mappedBy = "board") // 여기서 board는 Comments의 board와 변수명이 동일해야 한다.
    private List<Comments> commentList = new ArrayList<>();
    ...
}

public class Comments {
    @Id
    private Long comment_id;

    @ManyToOne
    @JoinColumn(name = "board_id")
    private Board board;
    ...
}

```

정리하면 mappedBy는 DB관점에서는 존재하지 않고, OOP관점에서만 존재하는 연관관계이다.

Board Entity의 Comments 맴버변수(private List<Comments> commentList;)에는 Comments Entity의 Board 맴버변수를 매핑하여 두 Entity간의 연관관계를 설정하게 한다.


## 3. 요청/응답 DTO 생성
`main > java > 프로젝트명 > dto` 폴더에 BoardReqDto.java 파일 생성
```java
@Getter
@Setter
public class BoardReqDto {
    private String author;
    private String subject;
    private String content;
    private String password;
}
```

`main > java > 프로젝트명 > dto` 폴더에 BoardResDto.java 파일 생성
```java
@Getter
@Setter
@Builder
public class BoardResDto {
    private Long id;
    private String author;
    private String subject;
    private String content;
    private LocalDateTime createdTime;
    private LocalDateTime modifiedDate;
    private int hitCount;
    private boolean delYn;
}

```

## 4. BoardRepository 생성
`main > java > 프로젝트명 > repository` 폴더에 BoardRepository.java 파일 생성

```java
@RestController
@RequestMapping("/board")
public class BoardController {

    @GetMapping("/hello")
    public String hello() {
        return "hello world";
    }

    @PostMapping
    public BoardResDto postBoard(@RequestBody BoardReqDto boardReqDto) {
        return "";
    }

    @PutMapping("/update")
    public String adjustBoard() {
        return "";
    }

    @PutMapping("/delete")
    public String deleteBoard() {
        return "";
    }
}
```
서버 구동 후 localhost:8080/board/hello로 접속 -> hello world 메시지 확인

끄읏.
---
layout: post
title:  "spring boot ConfigurationProperties로 application 변수 사용하기"
date:   2020-02-01 21:32:00 +0900
categories: springboot intellij ConfigurationProperties
---

## 1. properties에 데이터 입력
application.properties 또는 application.yml파일에 필요한 값을 입력한다.
- yml 설정 예시
```yaml
company:
    name: "EXBI"
    address: "서울"
    tel-number: "02-1234-1234"
    email: "master@exbi.co.kr"
```
- properties 설정 예시
```
company.name=EXBI
company.address=서울
company.tel-number=02-1234-1234
company.email=master@exbi.co.kr
```

## 2. 값을 바인딩할 component 생성
`@Component`, `@ConfigurationProperties`을 적용하고, prefix값으로 `company`를 지정했다. Spring container가 properties파일을 읽고 Company객체에 값을 바인딩할 때 Setter를 이용하기 때문에 `@Setter`도 함께 선언해줘야 한다. 
```java
@Component
@ConfigurationProperties("company")
@Getter
@Setter
public class Company {
	// 바인드될 맴버변수를 선언
    private String name;
    private String address;
    private String telNumber;
    private String email;

	// 정상적으로 바인드 되었는지 확인하기 위해 toString을 재정의
    @Override
    public String toString() {
        return "Company{" +
                "name='" + name + '\'' +
                ", address='" + address + '\'' +
                ", telNumber='" + telNumber + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
```

## 3. 의존관계 설정
ConfigurationProperties를 추가하면 아래와 같은 메시지를 만나게 된다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-02-01_001.png)
Properies를 사용하기 위해 필요한 의존관계를 설정해야 한다.
- Maven

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-configuration-processor</artifactId>
	<optional>true</optional>
</dependency>
```

- Gradle 4.5 이하

```
dependencies {
	compileOnly "org.springframework.boot:spring-boot-configuration-processor"
}
```

- Gradle 4.6 이상

```
dependencies {
	annotationProcessor "org.springframework.boot:spring-boot-configuration-processor"
}
```

## 4. test case로 확인 해보기
```java
@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class CompanyTest {

    @Autowired
    Company company;

    @Test
    public void companyValueTest() {
        //given
        String name = "EXBI";
        String address = "서울";
        String telNumber = "02-1234-1234";
        String email = "master@exbi.co.kr";

        //when

        //then
        System.out.println("company.toString(): " + company);
        assertEquals(name, company.getName());
        assertEquals(address, company.getAddress());
        assertEquals(telNumber, company.getTelNumber());
        assertEquals(email, company.getEmail());
    }
}
```
테스트 실행 결과 properties의 값이 Company객체에 바인딩된 것을 확인할 수 있다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-02-01_002.png)
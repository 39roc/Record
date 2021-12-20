# Week1

<br>

### 프로젝트 환경설정

#### 프로젝트 생성

무엇을 요약해야 할지 모르겠네..

단순 프로젝트 세팅하는거라 할 얘기가 없다.

뭘 전달하면 좋을까..?

H2 데이터베이스 인메모리DB 를 활용한 편리한 환경 세팅?



#### 라이브러리 살펴보기

Gradle 의존 관계 살펴보기

```
./gradlew dependencies
./gradlew dependencies —configuration compileClasspath
```



로깅 관련 조사 해보기(스프링 부트 스타터 로깅)

스프링 부트는 모든 내부 로깅에 [Commons Logging](https://commons.apache.org/proper/commons-logging/)을 사용, 로그 구현체는 선택 가능하고, [Java Util Logging](https://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html), [Log4j2](	https://logging.apache.org/log4j/2.x/), [Logback](https://logback.qos.ch/) 을 위한 디폴트 설정 제공



스타터를 사용할 경우 기본적으로 로그백으로 로깅한다.



slf4j -> 인터페이스

logback -> 구현체, 디폴트

log4j -> 구현체



스프링부트 스타터test

기타 라이브러리

- H2
- 케넉션 풀: HikariCP 디폴트
- 로깅 Slf4j & logBack
- 테스트

스프링 데이터 JPA는 스프링과 JPA를 먼저 이해하고 사용해야 하는 응용기술이다.



#### View 환경 설정

JPA 를 학습하는 것이 목적이기때문에 타임리프 활용은 제외

#### H2 데이터베이스 설치

##### H2 Database 특징

- 임베디드, 서버 모드 지원 (디스크 기반 또는 인메모리 데이터베이스)
- 트랜잭션 지원
- 브라우저 기반 콘솔앱 지원
- 경량의 파일 사이즈
- Embedded and server modes; disk-based or in-memory

로컬 개발 환경 구성에 적합한DB, 다음의 [H2 Cheat Sheet](https://www.h2database.com/html/cheatSheet.html) 참고

키값 유지..?

jpashop.mv.db 파일생성 확인

파일 생성 이후 tcp로 연결

h2 에 대해 알아보기

h2쉘을 실행한채로? 

로컬 개발 환경 구성시 활용에 적합

#### JPA와 DB 설정, 동작확인

```
spring:
 datasource:
 	url: jdbc:h2:tcp://localhost/~/jpashop
 	username: sa
 	password:
 	driver-class-name: org.h2.Driver
 jpa:
 	hibernate:
 		ddl-auto: create
 	properties:
# show_sql: true
 format_sql: true

 
logging.level:
 org.hibernate.SQL: debug
# org.hibernate.type: trace

 참고: 모든 로그 출력은 가급적 로거를 통해 남겨야 한다.
> show_sql : 옵션은 System.out 에 하이버네이트 실행 SQL을 남긴다.
> org.hibernate.SQL : 옵션은 logger를 통해 하이버네이트 실행 SQL을 남긴다.
```



```
url: jdbc:h2:tcp://localhost/~/jpashop;MVCC=TRUE

MVCC 옵션에 대한 참고
Multi-Version Concurrency Control (MVCC)
Insert and update operations only issue a shared lock on the table. An exclusive lock is still used when adding or removing columns or when dropping the table. Connections only 'see' committed data, and own changes. That means, if connection A updates a row but doesn't commit this change yet, connection B will see the old value. Only when the change is committed, the new value is visible by other connections (read committed). If multiple connections concurrently try to lock or update the same row, the database waits until it can apply the change, but at most until the lock timeout expires.

H2 1.4.200 버전부터 MVCC 옵션 제거 됨
다음의 링크 참고
https://github.com/h2database/h2database/releases/tag/version-1.4.200

Alternative MVStore database

*.h2.db is a PageStore database. *.mv.db is a MVStore database.
```



- 하이버네이트에 관한 옵션은 공식문서 참고
  - [Hibernate 문서](https://docs.jboss.org/hibernate/stable/orm/userguide/html_single/Hibernate_User_Guide.html)
  - org.hibernate.cfg.Environment 클래스 주석 참고
- 스프링 부트 관련 Environment 구성 요소는 공식 문서 참고
  - [스프링 부트 공식 문서]()



@PersistenceContext 에 어떻게 주입 될까.?ㅣㅐ

레이어별 테스트 코드 

https://github.com/HomoEfficio/dev-tips/blob/master/Spring-Boot-%EB%A0%88%EC%9D%B4%EC%96%B4%EB%B3%84-%ED%85%8C%EC%8A%A4%ED%8A%B8.md



/ㅈ ㄹㅇ

- 엔티티 매니저를 통할 경우 트랜잭션 안에서 이뤄져야함

```
./gradle clean build
```



스프링부트를 통해 복잡한 설정 자동화, 순수 스프링 JPA 설정시 사용하던 persitence.xml , LocalContainerEntityManagerFactoryBean 도 없다.



##### 쿼리 파라미터 남기기

- org.hibernate.type: trace 옵션 주기
- 외부 라이브러리 사용
  - [스프링 데이터 소스 데코레이터](https://github.com/gavlyukovskiy/spring-boot-data-source-decorator)
  - [https://jessyt.tistory.com/27](https://jessyt.tistory.com/27)



### 도메인 분석 설계

#### 요구사항 분석

**기능 목록**

- 회원 기능
  - 회원 등록 
  - 회원 조회 
- 상품 기능 
  - 상품 등록 
  - 상품 수정 
  - 상품 조회 
- 주문 기능 
  - 상품 주문 
  - 주문 내역 조회 
  - 주문 취소 
- 기타 요구사항 
  - 상품은 재고 관리가 필요하다. 
  - 상품의 종류는 도서, 음반, 영화가 있다. 
  - 상품을 카테고리로 구분할 수 있다.
  - 상품 주문시 배송 정보를 입력할 수 있다.



#### 도메인 모델과 테이블 설계

연관 관계 주인에 대하여, 왜 왜래 키가 있는 쪽이 연관관계의 주인으로 정하는 것이 좋을까?



#### 엔티티 클래스 개발1

연관관계 주인, 외래키에 대해서 알아보기	



#### 엔티티 클래스 개발2

값 객체



#### 엔티티 설계시 주의점



### 애플리케이션 구현 준비

레이어드 아키텍처에 대해 알아보기




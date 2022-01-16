# QueryDsl





## 프로젝트 환경 설정

querydsl gradle 설정 공부해보기



## 예제 도메인 모델

도메인 모델 세팅



## 기본 문법

### JPQL vs Querydsl

쿼리문 잘못 작성했을때 오류발견 컴파일 vs 런타임



### 기본 QType

사용 방법 2가지

- alias
  - 같은 테이블을 조인해야 되는 경우
- static import



### 검색 조건 쿼리

Where 절 컨벤션 2가지

- and 명시적 표시
- 생략



### 결과조회

- `fetch()`
- `fetchOne()`
- `fetchFirst()`
- `fetchResults()`
  - 성능 중요한곳 사용 X -> 조사 필요



### 정렬



### 페이징



### 집합


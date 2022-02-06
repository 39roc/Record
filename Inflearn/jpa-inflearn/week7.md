# Week7



## API 개발 고급 - 지연 로딩과 조회 성능 최적화



### V1

> 응답으로 엔티티를 직접 외부에 노출한 경우



#### 문제

- 엔티티에 프레젠테이션 계층을 위한 로직 생성
- 영속계층 프레젠테이션 계층간의 의존성 문제
  - API 스펙과 엔티티 간의 서로 영향을 줌



#### 해결

- API 응답별 DTO 생성



### V2

> 응답으로 별도의 DTO 사용



- 엔티티가 변해도 API 스펙이 변하지 않음
- 엔티티를 DTO 변환해야 하는 매핑 로직 발생



:bulb: 지연로딩 기본, 최적화가 필요한 부분에만 페치 조인 사용

### V3

> 엔티티를 DTO로 변환 - 페치 조인 최적화



- V2에서 발생하던 N+1 문제 해결
- 한번의 쿼리로 연관관계 모두 조회



### V4

> JPA 에서 DTO 로 조회



- JPA에서 엔티티를 조회하는 것이 아닌 프로젝션 등의 기능을 통해 DTO 로 조회

- 애플리케이션 네트워크 용량 최적화

- 엔티티 아닌 DTO로 조회할 경우 재사용성 떨어지고, 특정 API 스펙에 의존



#### 
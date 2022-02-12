# Week8

<br>

## :bookmark_tabs: 컬렉션 조회 최적화

> :bulb: OneToMany 관계의 컬렉션 조회 케이스



### V1 엔티티 직접 노출

```java
@GetMapping("/api/v1/orders")
 public List<Order> ordersV1() {
	List<Order> all = orderRepository.findAll();
 	for (Order order : all) {
 		order.getMember().getName();
 		order.getDelivery().getAddress();
 		List<OrderItem> orderItems = order.getOrderItems();
 		orderItems.stream().forEach(o -> o.getItem().getName());
	 }
 	return all;
 }
```



#### 문제

- 도메인 엔티티와 프레젠테이션 계층 의존관계 문제
- 트랜잭션 지연로딩 필요
- JSON 변환과정 무한루프 발생 - 양방향일때 한쪽에 @JsonIgnore 필요

<br>

### V2 엔티티를 DTO 변환

```java
@GetMapping("/api/v2/orders")
public List<OrderDto> ordersV2() {
	List<Order> orders = orderRepository.findAll();
 	List<OrderDto> result = orders.stream()
 								.map(o -> new OrderDto(o))
								.collect(toList());
 	return result;
}
```



#### 문제

- 지연로딩 N+1 문제 발생

<br>

### V3 엔티티를 DTO 변환 - 페치 조인

```java
public List<Order> findAllWithItem() {
 return em.createQuery(
 	"select distinct o from Order o" +
 	" join fetch o.member m" +
 	" join fetch o.delivery d" +
 	" join fetch o.orderItems oi" +
 	" join fetch oi.item i", Order.class)
 	.getResultList();
}
```



#### 문제

- `distinct`  데이터베이스 키워드와 동일하지 않음
- 페이징 불가능 -> 전체 데이터를 메모리에 로딩해 애플리케이션상에서 페이징 처리 / 성능 이슈 발생



:bulb:

> 컬렉션 페치 조인 사용하면 페이징 불가능 또한 컬렉션 페치 조인은 1개만 사용 가능
>
> 위반시 [MultipleBagFetchException](https://jojoldu.tistory.com/457) 예외 발생

<br>



### V3.1 페이징 한계 해결

#### 해결

- XToOne 지연로딩  `hibernate.default_batch_fetch_size`  활용
  - DB에서 지원하는 IN 쿼리수 최대값 및 부하 고려하여 사이즈 결정
- XToOne 관계만 페치조인, 나머지는 IN 쿼리 조회 후 하이버네이트가 애플리케이션에서 자동 매핑

<br>

### V4 

```java
public List<OrderQueryDto> findOrderQueryDtos() {
 	//루트 조회(toOne 코드를 모두 한번에 조회)
 	List<OrderQueryDto> result = findOrders();
	//루프를 돌면서 컬렉션 추가(추가 쿼리 실행)
 	result.forEach(o -> {
 		List<OrderItemQueryDto> orderItems = findOrderItems(o.getOrderId());
 		o.setOrderItems(orderItems);
 	});
 return result;
}
```



#### 문제

- XToOne 페치 조인하여 DTO 로 조회, XToMany 추가 조회하여 애플리케이션에서 매핑
- XToMany 쿼리 여러번 발생 ( 루트 1번, 컬렉션 N번 )

<br>

### V5

```java
public List<OrderQueryDto> findAllByDto_optimization() {
	 //루트 조회(toOne 코드를 모두 한번에 조회)
 	List<OrderQueryDto> result = findOrders();
 	//orderItem 컬렉션을 MAP 한방에 조회
 	Map<Long, List<OrderItemQueryDto>> orderItemMap	=findOrderItemMap(toOrderIds(result));
 	//루프를 돌면서 컬렉션 추가(추가 쿼리 실행X)
 	result.forEach(o -> o.setOrderItems(orderItemMap.get(o.getOrderId())));
 return result;
}
```

#### 해결

- 루트 1번, 컬렉션 1번으로 최적화
- 매핑 위한 추가적인 로직 필요 단점



### V6

#### 해결

- 데이터들을 플랫하게 모두 조회한 다음에 애플리케이션에서 중복된 데이터들을 제거하고 매핑하는 로직을 추가로 구성



### OSIV와 성능 최적화

- 영속성 컨텍스트의 생존 범위
- `spring.jpa.open-in-view` 
- 데이터베이스 커넥션 리소스에 대한 고려



> :bulb: OSIV 옵션 false 로 두고 CQRS 활용




# 인덱스



### 목차

- 디스크 읽기 방식
- 인덱스란?
- B-Tree 인덱스
- 해시(Hash) 인덱스
- R-Tree 인덱스
- Fractal-Tree 인덱스
- 전문 검색(Full Text search) 인덱스
- 비트맵 인덱스와 함수 기반 인덱스
- 클러스터링 인덱스
- 유니크 인덱스
- 외래키



### 디스크 읽기 방식

컴퓨터의 CPU나 메모리와 같은 전기적 특성을 띤 장치의 성능은 짧은 시간 동안 빠르게 발전했지만 디스크와 같은 기계식 장치의 성능은 제한적으로 발전 그러므로 데이터베이스 성능은 디스크 I/O 와 관련이 많다.

<br>

#### 저장 매체

- 내장 디스크
- Direct Attached Strorage
- Network Attached Storage
- Storage Area Network

- 내장디스크 컴퓨터 내부 공간적 제약때문에 용량 문제 발생
- DAS 용량문제 해결하지만 하나의 본체에만 연결 디스크의 정보를 여러 컴퓨터가 동시에 공유 불가능하고 SATA , SAS 와 같은 케이블로 연결
- NAS  와 DAS 의 가장 큰 차이는 여러 컴퓨터에서 동시 사용 가능성과 컴퓨터 본체와 연결 방식
  - DAS 는 케이블 NAS 는 TCP/IP
- SAN 고가의 구축 비용

대부분의 저장 매체는 디스크 드라이브의 *플래터*를 회전 시켜서 데이터를 읽고 쓴다.

<br>

### 인덱스란

일반적으로 책의 색인과 비교하여 설명한다. SortedList 와 ArrayList 의 차이를 통해 인덱스를 생각해볼수 있다. 인덱스의 경우 SortedList를 떠올리면 된다. 데이터를 정렬된 상태로 유지한다.

SortedList 의 장점은 정렬된 상태로 있기 때문에 데이터 검색에 유리하며 단점으로는 매번 정렬을 유지해야하므로 정렬에 대한 비용이 든다. 그러므로 인덱스를 사용한다는 것은 Insert, Update, Delete 에 불리한 반면 Read 에 유리하다는점이다.

인덱스는 데이터를 관리하는 방식과 중복 값의 허용 여부에 따라 분류




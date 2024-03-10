# Spring-shop
 
Spring framework 스택을 익히기 위한 미니 쇼핑몰 프로젝트

기술
- Java 17, Spring Boot 3.2.0, Gradle 8.4, junit 4.13
- Spring Web MVC, Thymeleaf, Spring Data JPA, h2, IntelliJ

<img width="600" alt="image" src="https://github.com/hyeongseok21/spring-shop/assets/66310634/c2940767-3f66-4ba1-8342-f8da10544e3e">

기능 목록
- 회원 기능: 회원 등록, 회원 조회
- 상품 기능: 상품 등록, 상품 수정, 상품 조회
- 주문 기능: 상품 주문, 주문 내용 조회, 주문 취소
- 기타 요구사항: 상품 재고 관리, 상품 구분(도서, 영화, 음반), 주문 상태 구분(주문, 취소)

-------------------------------------------

### ERD
<img width="600" alt="image" src="https://github.com/hyeongseok21/spring-shop/assets/66310634/a8dbfe77-d2a5-4e76-a8c4-831eb457ee32">

1. 주문(Order)-상품(Item)의 Many-to-Many 관계를
- 주문(Order)-주문상품(OrderItem)의 One-to-Many,
- 주문상품(OrderItem)-상품(Item)의 Many-to-One 관계로 폴어냄
2. 주문이 Member_ID를 foreign key로 참조
- 회원이 주문을 하므로 회원이 주문 리스트를 가져야 할 것 같지만, 객체 세상에선 주문이 회원을 참조하는 것으로 충분

### Architecture
<img width="600" alt="image" src="https://github.com/hyeongseok21/spring-shop/assets/66310634/0cd20f09-f688-43f6-9526-17ab4484cfc0">

계층별 역할이 정확히 나누어지도록 설계
1. Controller: 웹 계층
2. Service: 비즈니스 로직, 트랜잭션 처리
3. Repository: 엔티티 매니저 사용, JPA에서 직접 사용
4. Domain: 엔티티의 집합. 모든 계층에서 사용


### 구현 상세
대원칙: 기능 하나당 테스트 코드를 작성해 TDD 적용
1. 주문 서비스의 order()와 cancleOrder()에 도메인 모델 패턴 적용, 서비스 계층 간소화를 통한 유지보수성 증가
2. 주문 검색 기능 구현을 위해 JPA에서 동적 쿼리 생성 필요 -> JPQL 사용
3. Post후 새로고침시 null data가 계속 등록 -> PRG 적용으로 해결
4. 회원 목록 조회 기능을 위해 Entity 직접 노출 대신 MemberForm 객체를 사용 -> 엔티티와 화면을 위한 로직 분리
5. 상품 수정 시 itemId 파라미터의 준영속 상태 처리 문제 -> 변경 감지를 사용해 트랜잭션이 있는 서비스 계층에서 영속 상태 엔티티를 조회 및 변경
# TDD

## 프로젝트 개요
- **프로젝트명**: TDD(Today Delicious Delivery)
- **기간**: 2025.9.26 ~ 2025.10.17 (3주)
- **팀 구성**: 5명
- **담당 역할**: Point, Store 도메인 설계 및 구현
- **프로젝트 성격**: Spring Boot 모놀리식 기반 **주문 관리 플랫폼**


## 소개
TDD(Today Delicious Delivery)는 '오늘의 맛있는 배달' 서비스로, Test 코드 작성 및 학습에도 비중을 두며 개발을 진행한 주문 관리 플랫폼입니다.  
전반적인 내용과 제가 담당했던 핵심 역할을 **정리**하고, 개발 과정에서의 **기술적 회고**를 작성하였습니다.  

기존 Repository 주소: https://github.com/Sparta-21/TDD

## 담당 영역
- **Point Domain**: 포인트 적립 및 사용 관리 서비스
- **Store Domain**: 가게 등록, 조회, 검색 관리 서비스

## 핵심 구현 기술

### 1. Spring AOP 기반 포인트 시스템 자동화
- **@Aspect**와 **@AfterReturning**을 활용하여 결제 완료 및 리뷰 작성 시 **포인트 적립을 자동화**
- 결제 취소 시에도 AOP를 통해 **적립 대기 포인트를 자동 회수**하여 데이터 정합성 유지
- Payment, Review 서비스 코드에서 포인트 관련 로직이 완전히 제거되어 **단일 책임 원칙(SRP) 준수**

### 2. QueryDSL 기반 동적 쿼리 및 N+1 문제 해결
- **2단계 쿼리 전략**을 적용하여 페이징이 적용된 `storeId` 목록을 먼저 조회한 뒤, IN 절로 연관 데이터를 일괄 로딩하여 **쿼리 호출 횟수를 3회로 최소화**
- **Store 엔티티에 `avgRating`, `reviewCount` 필드를 비정규화**하여 저장함으로써 **N+1 문제를 사전에 예방**하고 읽기 성능 향상
- **컴파일 시점**에 쿼리 문법 오류를 사전에 포착하여 런타임 안정성 확보
- **BooleanBuilder**와 Where 절을 활용하여 가독성 높고 확장이 용이한 검색 리포지토리 구축

### 3. Mockito 기반 단위 테스트
- **Mockito**를 활용하여 Service Layer의 의존성을 Mocking하고 독립적인 단위 테스트 환경 구축
- Given-When-Then 패턴을 일관되게 적용하여 테스트 코드의 가독성 향상
- Point, Store 서비스의 핵심 비즈니스 로직에 대한 단위 테스트 작성

### 4. JPA Auditing & Soft Delete 패턴
- JPA Auditing과 Spring Security를 통합하여 **데이터 생성/수정 시점과 주체를 자동으로 추적**
- 데이터 생성/수정 시점과 주체를 **자동으로 추적**하고, 수동 설정 없이 감사 정보가 기록되어 **휴먼 에러 방지**
- **Soft Delete 패턴**을 도입하여 데이터를 논리적으로 삭제 표시함으로써, 삭제된 데이터의 **복구 가능성** 확보


## 기술 스택
| Category | Technology |
|----------|--------|
| Language | Java |
| Framework | Spring Boot, Spring Security |
| ORM | Spring Data JPA, QueryDSL|
| Database | PostgreSQL|
| Build Tool | Gradle |
| Infra | Docker Compose, Github Actions(CI) |
| Open API | Google GenAI API, Naver Map API |
| Testing | JUnit5, Mockito, TestContainers |
| API Docs | Swagger |


## 아키텍처
```
├── domain                 
│   ├── point            
│   │   ├── entity         
│   │   ├── dto           
│   │   ├── repository     
│   │   ├── service        
│   │   └── enums          
│   ├── store             
│   │   ├── entity         
│   │   ├── dto            
│   │   ├── repository     
│   │   └── service        
│   ├── user               
│   ├── order             
│   ├── payment            
│   └── ...               
└── global                 
    ├── exception         
    ├── config             
    └── common             
```



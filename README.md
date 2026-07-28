# 강병호

Spring 기반 서비스를 직접 설계하고 운영 환경까지 구축하며, 장애 상황과 데이터 정합성을 고려하는 백엔드 개발자입니다.

**Java Backend**

---

## Tech Stack

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/SpringBoot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/SpringDataJPA-6DB33F?style=flat-square&logo=spring&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=flat-square&logo=apache-maven&logoColor=white)

---

## Experience

### NHN Academy Java Backend 개발자 과정 12기
**2025.07.28** ~ **2025.12.31** 수료

---

## Projects

### Relay - 실시간 메신저 서버
**2026.03.30** ~ **2026.06.30** | 개인 프로젝트

실시간 메신저 서비스로, 동시성, 실시간 메시지 처리, 트랜잭션, 데이터 모델링 등 백엔드 기본기를 깊게 다질 수 있는 주제로 직접 선택해 설계부터 AWS 배포까지 구현한 개인 프로젝트입니다.

프론트엔드는 Next.js + TypeScript로 구현하여 백엔드와 연동했습니다.

**기술 스택**: Java 25, Spring Boot 4, Spring Data JPA, QueryDSL, MySQL, Redis Pub/Sub, WebSocket/STOMP, Testcontainers, TypeScript, React 19, Next.js 16

#### 도메인
* **[Messenger Server](https://www.splleat.com/)**

#### Repository
* **[Front-end](https://github.com/Splleat/Messenger-Front)**
* **[Back-end](https://github.com/Splleat/Messenger-Project)**

### 주요 기술적 의사결정

1. **TSID 도입** - Auto Increment, UUIDv4, UUIDv7을 ID 생성 방식, 정렬 특성, 인덱스 효율, 저장 공간 관점에서 비교한 뒤 DB BIGINT와 호환되면서 시간 순 정렬이 가능한 TSID를 선택. 도입 과정에서 발생한 JPA save() 추가 SELECT 쿼리 문제와 JavaScript number 정밀도 문제를 해결
2. **JWT + Redis Blacklist**: JWT의 Stateless 특성과 로그아웃 요구사항 사이의 트레이드오프를 분석하고 Redis Blacklist를 적용
3. **커서 기반 양방향 페이징**: TSID의 시간 정렬 특성을 활용해 (channel_id, id) 복합 인덱스 기반의 메시지 조회 및 양방향 커서 페이징 구현

#### 문서
* [JWT는 정말로 Stateless한가?](https://github.com/Splleat/Messenger-Project/blob/dev/docs/01-jwt-stateless.md)
* [TSID를 도입하면서 마주친 JPA save 전략과 JavaScript 정밀도 문제](https://github.com/Splleat/Messenger-Project/blob/dev/docs/02-db-pk.md)
* [커서 기반 양방향 페이징 설계](https://github.com/Splleat/Messenger-Project/blob/dev/docs/03-paging-strategy.md)

### MSA 기반 온라인 서점 'Trillion'
**2025.11.11 ~ 2025.12.31** | 팀 프로젝트 (주문 도메인 담당)

8인 팀 프로젝트로, 주문 도메인 설계 및 분산 트랜잭션 설계를 담당했습니다.

**기술 스택**

Java, Spring Boot, Spring Data JPA, MySQL, OpenFeign, Resilience4j

#### 도메인
* **[Trillion-book](https://trillion-book.shop/)**

#### Repository
* **[Trillion](https://github.com/nhnacademy-be12-trillion)**
* **[주문 서비스](https://github.com/Splleat/Trillion-Order)**

### 주요 기술적 의사결정

1. **오케스트레이션 Saga 패턴**: Database per Service 환경에서 도서 재고, 쿠폰, 포인트 서비스에 걸친 분산 트랜잭션을 오케스트레이션 Saga로 설계하고 비즈니스 성격에 따라 롤백과 재시도 전략을 분리
2. **보정 스케줄러 + ShedLock**: 서버 장애로 중단된 Saga 트랜잭션을 자동 복구하고, RDB 기반 ShedLock으로 서버 이중화 환경의 스케줄러 중복 실행을 방지

#### 문서
*   [분산 트랜잭션과 Saga Pattern 설계](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/01-saga-pattern.md)
*   [보상 트랜잭션은 어떻게 보상하나 - Saga 보상 스케줄러 설계](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/02-scheduling.md)

---

## Contact
- **E-mail**: [Splleat@gmail.com](mailto:Splleat@gmail.com)

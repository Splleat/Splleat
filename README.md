# 강병호

데이터 정합성과 시스템 안정성에 관심이 많은 백엔드 개발자입니다.

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
**2026.03.30** ~ 진행 중 | 개인 프로젝트

실시간 메시지 송수신과 메시지 이력 탐색을 목표로 설계한 메신저 서버입니다. 인증, 메시징, 데이터 조회 성능을 직접 검증하며 운영 환경까지 구축했습니다.

프론트엔드는 Next.js + TypeScript로 구현하여 백엔드와 연동했습니다.

**기술 스택**: Java 25, Spring Boot 4, Spring Data JPA, QueryDSL, MySQL, Redis Pub/Sub, WebSocket/STOMP, Testcontainers

#### 도메인
* **[Messenger Server](https://www.splleat.com/)**

#### Repository
* **[Front-end](https://github.com/Splleat/Messenger-Front)**
* **[Back-end](https://github.com/Splleat/Messenger-Project)**

### 주요 기술적 의사결정

1. **TSID 도입** - Auto Increment의 분산 DB 한계와 예측 가능성 문제, UUIDv4의 인덱스 성능 문제, UUIDv7의 인덱스 크기 문제를 검토한 끝에 DB BIGINT와 호환이 가능한 TSID 선택. 도입 과정에서 발생한 JPA `save()` 동작 문제와 JS `number` 정밀도 문제를 해결
2. **JWT + Redis Blacklist**: Stateless 특성 때문에 JWT를 선택했지만 로그아웃 구현 과정에서 모순을 체감하고 세션 방식과 실질적인 트레이드오프를 비교
3. **커서 기반 양방향 페이징**: TSID의 시간 정렬 특성을 활용해 과거 / 최신 메시지 페이징과 채널 입장 메시지 조회를 `(channel_id, id)` 복합 인덱스로 구현

#### 문서
* [JWT는 정말로 Stateless한가?](https://github.com/Splleat/Messenger-Project/blob/dev/docs/01-jwt-stateless.md)
* [TSID를 도입하면서 마주친 JPA Persist 전략과 JavaScript 정밀도 문제](https://github.com/Splleat/Messenger-Project/blob/dev/docs/02-db-pk.md)
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

1. **오케스트레이션 사가 패턴**: Database per Service 환경에서 도서 재고/쿠폰/포인트 서비스에 걸친 분산 트랜잭션 정합성 보장. 주문 생성 실패 시 롤백, 주문 취소는 재시도로 비즈니스 성격에 따라 다르게 적용
2. **보정 스케줄러 + ShedLock**: 서버 장애로 중단된 사가 트랜잭션을 자동 복구하는 시스템 구현. Redis 없이 기존 RDB만으로 분산 락 구현

#### 문서
*   [분산 트랜잭션과 Saga Pattern 설계](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/01-saga-pattern.md)
*   [보상 트랜잭션은 어떻게 보상하나 - Saga 보상 스케줄러 설계](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/02-scheduling.md)

---

## Contact
- **E-mail**: [Splleat@gmail.com](mailto:Splleat@gmail.com)

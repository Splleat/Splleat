# 강병호

실패 흐름까지 생각하는 백엔드 개발자입니다.

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?style=flat-square&logo=kotlin&logoColor=white)
![Spring Boot](https://img.shields.io/badge/SpringBoot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/SpringDataJPA-6DB33F?style=flat-square&logo=spring&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHubActions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

| 프로젝트               | 소개                     | 역할               |                                                                             |
|--------------------|------------------------|------------------|-----------------------------------------------------------------------------|
| **Trillion**       | MSA 온라인 서점 · 8인 팀      | 주문 도메인과 분산 트랜잭션  | [저장소](https://github.com/Splleat/Trillion-Order)                            |
| **Relay (Java)**   | 실시간 메신저 · 개인           | 설계부터 프론트엔드와 배포까지 | [저장소](https://github.com/Splleat/Messenger-Project)                         |
| **iUnoT**          | MSA 의약품 재고관리 · 8인 팀    | 인프라, 배포, 모니터링    | [저장소](https://github.com/nhnacademy-aiot3-iUnoT)                            |
| **Relay (Kotlin)** | 메신저 중심 협업툴 · 개인 · 진행 중 | 백엔드와 React 프론트엔드 | [서비스](https://relay.splleat.com/) · [저장소](https://github.com/Splleat/relay) |

---

## Trillion — 온라인 서점 서비스

**2025.11.11 ~ 2025.12.31**

8인 팀 프로젝트로, 주문 도메인을 담당해 MSA 환경에서 분산 트랜잭션을 경험했습니다.

**역할**
- 주문 생성 / 취소 / 환불 비즈니스 로직 구현
- Saga 패턴 기반 주문 흐름 설계
- 중단된 주문을 자동으로 복구하는 복구 스케줄러 구현

**문서** [MSA에서의 분산 트랜잭션과 사가 패턴 적용기](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/01-saga-pattern.md) · [보상 트랜잭션은 어떻게 보상하나?](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/02-scheduling.md)

**기술** Java/Spring, Spring Data JPA, MySQL, OpenFeign, Resilience4j, ShedLock

**저장소** [nhnacademy-be12-trillion](https://github.com/nhnacademy-be12-trillion) (팀) / [Trillion-Order](https://github.com/Splleat/Trillion-Order) (주문)

---

## Relay — 실시간 메신저 서비스

**2026.03.30 ~ 2026.06.30**

개인 프로젝트로, 설계부터 배포까지 경험했습니다.

**역할**
- WebSocket + Redis Pub/Sub 기반 실시간 메시지 처리
- TSID와 (channel_id, id) 복합 인덱스 하나로 커서 페이징 구현
- JWT 기반 인증 구현
- 객체 스토리지로 첨부파일 기능 구현
- 컨테이너 기반 테스트 환경 구축
- Next.js 기반 프론트엔드 구현 및 연동
- AWS 배포

**문서** · [JWT는 정말로 Stateless한가?](https://github.com/Splleat/Messenger-Project/blob/dev/docs/01-jwt-stateless.md) · [TSID를 도입하며 만난 JPA 영속성 전략과 JavaScript 정밀도 문제](https://github.com/Splleat/Messenger-Project/blob/dev/docs/02-db-pk.md)

**기술** Java/Spring, Spring Data JPA, QueryDSL, MySQL, Redis, WebSocket/STOMP, Testcontainers, TypeScript, Next.js/React, AWS (EC2, RDS, S3)

**저장소** [Messenger-Front](https://github.com/Splleat/Messenger-Front) (프론트엔드) / [Messenger-Project](https://github.com/Splleat/Messenger-Project) (백엔드)

---

## iUnoT — 의약품 재고관리 서비스

**2026.07.02 ~ 2026.09.17**

8인 팀 프로젝트로, 인프라를 담당해 MSA 환경에서 컨테이너 기반 무중단 배포와 모니터링 환경을 구성했습니다.

**역할**
- 롤링 배포로 배포 중 요청 끊김 제거
- 로그 수집/모니터링 환경 구성
- 에러 로그를 LLM으로 요약해 텔레그램으로 전송하는 알림 서버 개발
- GitHub Actions로 CI/CD 자동화 및 PR 알림

**기술** Java/Spring, Spring Cloud Gateway, Netflix Eureka, Cloudflare Tunnel, Nginx, Prometheus, Filebeat, Logstash, ElasticSearch, Grafana, Zipkin, Docker, Spring AI

**저장소** [nhnacademy-aiot3-iUnoT](https://github.com/nhnacademy-aiot3-iUnoT) (팀) / [msa-infra](https://github.com/nhnacademy-aiot3-iUnoT/msa-infra) · [infra](https://github.com/nhnacademy-aiot3-iUnoT/infra) · [alert](https://github.com/nhnacademy-aiot3-iUnoT/alert) (인프라)

---

## Education

- **NHN Academy 인공지능 기반 IoT 웹서비스 개발자 양성과정 3기** · 2025.12.23 ~ 2026.09.21 수료
- **NHN Academy Java Backend 개발자 과정 12기** · 2025.07.28 ~ 2025.12.31 수료

## Contact

[splleat@gmail.com](mailto:splleat@gmail.com)

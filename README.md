# 강병호

정상 흐름뿐만 아니라 실패 흐름까지 생각하는 백엔드 개발자입니다.

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/SpringBoot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/SpringDataJPA-6DB33F?style=flat-square&logo=spring&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/SpringCloud-6DB33F?style=flat-square&logo=spring&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHubActions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

| 프로젝트 | 무엇 | 맡은 것 | |
|---|---|---|---|
| **Trillion** | MSA 온라인 서점 · 8인 팀 | 주문 도메인과 분산 트랜잭션 | [저장소](https://github.com/Splleat/Trillion-Order) |
| **Relay** | 실시간 메신저 · 개인 | 설계부터 프론트엔드와 배포까지 | [서비스](https://www.splleat.com/) · [저장소](https://github.com/Splleat/Messenger-Project) |
| **iUnoT** | MSA 의약품 재고관리 · 8인 팀 | 인프라, 배포, 모니터링 | [저장소](https://github.com/nhnacademy-aiot3-iUnoT) |

---

## Trillion — 주문 도메인

**2025.11.11 ~ 2025.12.31** · 8인 팀 프로젝트

주문 하나를 만들려면 도서 재고, 쿠폰, 포인트 서비스를 모두 거쳐야 하는데 서비스마다 DB가 나뉘어 있어 한 트랜잭션으로 묶을 수 없었습니다. 중앙에서 순서를 제어하고 실패하면 역순으로 되돌리는 오케스트레이션 Saga로 설계했습니다.

**문서** · [분산 트랜잭션과 Saga 패턴 설계](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/01-saga-pattern.md) · [보상 트랜잭션은 어떻게 보상하나](https://github.com/Splleat/Trillion-Order/blob/main/docs/wiki/02-scheduling.md)

- **5xx는 실패인가?** — 재고 차감 요청에 5xx가 오면 차감 후 응답만 유실된 것인지 차감 전에 죽은 것인지 구분할 수 없었습니다. 처리 여부를 모르는 데이터를 남기는 것보다 항상 되돌리는 편이 낫다고 보고, 처리되지 않은 요청까지 되돌리는 낭비를 감수했습니다. 대신 받는 쪽에서 요청 식별자로 이미 처리한 요청인지 확인해 한 번만 처리하도록 했습니다.
- **주문 서버가 멈추는 경우** — 주문 서버 자체가 멈추면 보상을 실행할 수 없습니다. 장애 시 복구 이벤트를 발행하는 방법은 서버가 멈추면 불가능하므로, DB에 커밋된 사가 기록을 기준으로 5분 주기 스케줄러가 정리하도록 했습니다. 이중화 환경의 중복 실행은 RDB 분산 락으로 막았습니다.

**기술** Java 21, Spring Boot 3, Spring Data JPA, MySQL, OpenFeign, Resilience4j, ShedLock

**팀 저장소** [nhnacademy-be12-trillion](https://github.com/nhnacademy-be12-trillion)

---

## Relay — 실시간 메신저

**2026.03.30 ~ 2026.06.30** · 개인 프로젝트

실시간 메시지 처리와 트랜잭션, 데이터 모델링을 한 번에 다룰 수 있는 주제로 골랐습니다. 도메인 설계부터 AWS 배포까지 직접 했고, 확인할 화면이 필요해 프론트엔드도 Next.js로 만들어 Vercel에 배포했습니다. 메시지와 발행 이벤트를 한 트랜잭션에 저장하는 아웃박스 패턴으로 Redis 발행 실패에 대비했습니다.

**문서** · [JWT는 정말로 Stateless한가?](https://github.com/Splleat/Messenger-Project/blob/dev/docs/01-jwt-stateless.md) · [TSID를 도입하며 만난 예상치 못한 문제들](https://github.com/Splleat/Messenger-Project/blob/dev/docs/02-db-pk.md) · [커서 기반 양방향 페이징 설계](https://github.com/Splleat/Messenger-Project/blob/dev/docs/03-paging-strategy.md)

- **애플리케이션 레벨 ID 생성의 오해** — PK를 TSID로 정하면서, 애플리케이션에서 ID를 직접 만들면 JPA가 저장할 때마다 조회를 한 번 더 실행한다고 알고 있었습니다. 하지만 직접 재현해보니 ID 생성기로 생성한 TSID는 쿼리가 한 건이었고, 직접 생성해 할당한 경우에만 SELECT 쿼리가 추가로 발생해 두 건이었습니다. 애플리케이션에서 ID를 생성하더라도, ID 생성기가 Hibernate에 등록되어 있다면 추가 쿼리가 발생하지 않았던 것입니다. 알려진 설명이라도 직접 확인해야 한다는 것을 이때 알게 되었습니다.
- **JWT의 무상태를 일부 포기** — 서버가 상태를 갖지 않는다는 이유로 JWT를 골랐는데, 로그아웃을 구현하려니 발급한 토큰을 만료 전에 무효화해야 했습니다. 토큰 유효 기간을 짧게 두고 무효 토큰만 Redis에 보관하는 절충안을 택했고, JWT와 세션 방식을 다시 비교하며 생각한 것을 문서로 남겼습니다.

**기술** Java 25, Spring Boot 4, Spring Data JPA, QueryDSL, MySQL, Redis Pub/Sub, WebSocket/STOMP, Testcontainers, TypeScript, React 19, Next.js 16

**프론트엔드** [Messenger-Front](https://github.com/Splleat/Messenger-Front)

---

## iUnoT — 인프라

**2026.07.02 ~ 2026.09.17** · 8인 팀 프로젝트

의약품 창고의 입출고와 재고, 보관 환경을 관리하는 서비스입니다. 클라우드가 아니라 여러 팀이 함께 쓰는 온프레미스 서버라, 열어둔 포트나 디스크 사용이 다른 팀에도 영향을 주는 환경이었습니다. 외부에 포트를 열지 않고 요청을 받도록 Cloudflare Tunnel과 Nginx, Spring Cloud Gateway로 진입 경로를 나눴습니다.

**문서** · [API Gateway 패턴](https://github.com/nhnacademy-aiot3-iUnoT/msa-infra/blob/main/docs/api-gateway-pattern.md)

- **배포 중에 요청이 끊기던 문제** — 컨테이너를 내리고 새로 올리는 방식이라 배포하는 동안 요청이 그대로 실패했습니다. 서비스를 이중화한 뒤 레지스트리에서 인스턴스를 먼저 제외하고 한 대씩 교체하도록 바꿨고, 새 버전이 정상 응답하지 못하면 직전 성공 태그로 되돌아가게 했습니다. 배포 도중 k6로 3분간 540건을 보내 5xx와 연결 끊김이 0건임을 확인했습니다.
- **알림에 로그 본문을 담을 수 없던 문제** — 발생한 에러를 즉시 알고 싶었지만 ElasticSearch Watcher는 제공받은 환경에서 쓸 수 없었고 Grafana Alerting은 알림에 로그 본문을 담지 못했습니다. Logstash가 에러 로그 수집 시 알림 서버를 호출하도록 붙이고, 스택 트레이스를 정제한 뒤 LLM 요약을 거쳐 Telegram으로 보냈습니다. 요약이 실패해도 알림 자체는 발송되도록 분리했습니다.

**기술** Java 21, Spring Boot 4, Spring Cloud Gateway, Netflix Eureka, Cloudflare Tunnel, Nginx, Prometheus, Filebeat, Logstash, ElasticSearch, Grafana, Zipkin, Docker, Spring AI

**저장소** [msa-infra](https://github.com/nhnacademy-aiot3-iUnoT/msa-infra) · [infra](https://github.com/nhnacademy-aiot3-iUnoT/infra) · [alert](https://github.com/nhnacademy-aiot3-iUnoT/alert)

---

## Education

- **NHN Academy 인공지능 기반 IoT 웹서비스 개발자 양성과정 3기** · 2025.12.23 ~ 2026.09.21 수료
- **NHN Academy Java Backend 개발자 과정 12기** · 2025.07.28 ~ 2025.12.31 수료

## Contact

[splleat@gmail.com](mailto:splleat@gmail.com)

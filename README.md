# 강병호

**Java Backend**

---

## Tech Stack

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/SpringBoot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/SpringDataJPA-6DB33F?style=flat-square&logo=spring&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=flat-square&logo=apache-maven&logoColor=white)

---

## Experience
### NHN Academy Java Backend 개발자 과정 12기
**2025.07.28** ~ **2025.12.31**

---

## Projects
### MSA 기반 온라인 서점 'Trillion'
**2025.11.11 ~ 2025.12.31**

**담당 역할**: 주문(Order) 도메인 총괄 설계 및 구현

#### Repository
* **[Trillion Repository](https://github.com/nhnacademy-be12-trillion)**
* **[주문(Order) Repository](https://github.com/nhnacademy-be12-trillion/order/tree/dev)**

#### 기술적 의사결정
1.  **Saga Pattern 기반의 분산 트랜잭션 처리**
    *   **문제:** 각 서비스가 DB를 따로 가지는(Database per Service) MSA 환경에서는 단일 트랜잭션(ACID)을 사용할 수
       없음.
    *   **해결:** **Orchestration Saga Pattern**을 도입하여, `재고 차감` → `쿠폰 적용` → `포인트 사용`으로 이어지는
         복잡한 비즈니스 흐름의 데이터 정합성을 보장.
2.  **데이터 정합성을 위한 자가 치유 시스템**
    *   **문제:** 서버 장애 시 중단된 트랜잭션으로 인한 데이터 불일치 발생.
    *   **해결:** **ShedLock** 기반의 보정 스케줄러를 구현하여, 인프라 복잡도는 낮추면서도 중단된 사가(Saga)를 자동으로 복구하도록 설계.
3.  **장애 전파 차단**
    *   **문제:** HTTP 기반의 동기 통신(FeignClient) 환경에서 외부 서비스의 장애나 응답 지연 발생 시, 요청 스레드가 무한정 대기하며 스레드 풀(Thread Pool)이 고갈되어 주문 서비스 전체가 마비되는 연쇄 장애(Cascading Failure) 위험 노출.
    *   **해결:**
        1. **타임아웃(Timeout):** 연결(2초) 및 읽기(5초) 타임아웃을 짧게 설정하여 지연 발생 시 즉각 리소스를 반환하도록 튜닝.
        2. **재시도(Retry):** 일시적인 네트워크 깜빡임(Transient Fault)은 멱등성이 보장되게 설계된 API를 최대 3회(최초 시도 1회 + 재시도 2회) 재시도하여 가용성 확보.
        3. **서킷 브레이커(Circuit Breaker):** 반복적인 실패나 지연 감지 시 즉시 요청을 차단(Open)하고 빠른 실패(Fail-Fast)를 반환하여 장애 전파로부터 시스템을 보호.
    
#### 문서
*   [분산 트랜잭션과 Saga Pattern 설계](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Saga-Pattern.md)
*   [데이터 정합성 복구를 위한 스케줄링 전략](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Scheduling.md)
*   [장애 전파 차단과 Resilience4j 설정 전략](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Resilience4j.md)


---

## Contact
- **E-mail**: [Splleat@gmail.com](mailto:Splleat@gmail.com)

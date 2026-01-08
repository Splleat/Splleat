# 강병호

**Java Backend**

---

## Tech Stack

### Languages
![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)

### Frameworks
![Spring Boot](https://img.shields.io/badge/spring_boot-%236DB33F.svg?style=for-the-badge&logo=springboot&logoColor=white)
![Spring Data JPA](https://img.shields.io/badge/Spring_Data_JPA-%236DB33F.svg?style=for-the-badge&logo=spring&logoColor=white)

### Database
![MySQL](https://img.shields.io/badge/mysql-%234479A1.svg?style=for-the-badge&logo=mysql&logoColor=white)

### Build Tool
![Maven](https://img.shields.io/badge/maven-%23C71A36.svg?style=for-the-badge&logo=apache-maven&logoColor=white)

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
    *   **해결:** **Resilience4j Circuit Breaker**를 적용하여 외부 서비스(회원, 도서, 쿠폰) 장애 시 주문 시스템이 마비되는 연쇄 장애(Cascading Failure)를 방지하고 빠른 실패(Fail-Fast)를 유도.

#### 문서
*   [분산 트랜잭션과 Saga Pattern 설계](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Saga-Pattern.md)
*   [데이터 정합성 복구를 위한 스케줄링 전략](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Scheduling.md)
*   [장애 전파 차단과 Resilience4j 설정 전략](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Resilience4j.md)


---

## Contact
- **E-mail**: [Splleat@gmail.com](mailto:Splleat@gmail.com)

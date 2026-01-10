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

**담당 역할**: 주문(Order) 도메인 설계 및 분산 트랜잭션 아키텍처 구축

#### Repository
* **[Trillion Repository](https://github.com/nhnacademy-be12-trillion)**
* **[주문(Order) Repository](https://github.com/nhnacademy-be12-trillion/order/tree/dev)**

#### 기술적 의사결정
1.  **Saga Pattern 기반의 분산 트랜잭션 처리**
    *   **문제:** 각 서비스가 DB를 따로 가지는(Database per Service) MSA 환경에서는 단일 트랜잭션(ACID)을 사용할 수
       없음.
    *   **해결:** **Orchestration Saga Pattern**을 도입하여, `재고 차감` → `쿠폰 적용` → `포인트 사용`으로 이어지는
        복잡한 비즈니스 흐름의 데이터 정합성을 보장.
    <details>
    <summary>[주문 생성 다이어그램]</summary>
    
    ```mermaid
    sequenceDiagram
    autonumber
    participant Order as 주문 서비스
    box 외부 서비스
    participant Book as 도서 서비스
    participant Coupon as 쿠폰 서비스
    participant Member as 회원 서비스
    end
        
        Note over Order, Member: [Step 1] 재고 처리
        rect rgba(0, 255, 0, 0.1)
            Order->>Book: 재고 차감 요청
        end
        
        alt 재고 차감 실패
            rect rgba(255, 0, 0, 0.1)
                Book-->>Order: 에러 응답 (4xx/5xx)
                Order->>Book: 재고 복구
                Note over Order: 주문 실패
            end
        else
            Note over Order, Member: [Step 2] 쿠폰 처리
            rect rgba(0, 255, 0, 0.1)
                Order->>Coupon: 쿠폰 적용 요청
            end
        
            alt 쿠폰 적용 실패
                rect rgba(255, 0, 0, 0.1)
                    Coupon-->>Order: 에러 응답 (4xx/5xx)
                    Order->>Coupon: 쿠폰 취소
                    Order->>Book: 재고 복구
                    Note over Order: 주문 실패
                end
            else
                Note over Order, Member: [Step 3] 포인트 처리
                rect rgba(0, 255, 0, 0.1)
                    Order->>Member: 포인트 사용 요청
                end
        
                alt 포인트 사용 실패
                    rect rgba(255, 0, 0, 0.1)
                        Member-->>Order: 에러 응답 (4xx/5xx)
                        Order->>Member: 포인트 환불
                        Order->>Coupon: 쿠폰 취소
                        Order->>Book: 재고 복구
                        Note over Order: 주문 실패
                    end
                else
                    Note over Order: 주문 생성 완료
                end
            end
        end
    ```
    </details>
2.  **데이터 정합성을 위한 자가 치유 시스템**
    *   **문제:** 서버 장애 시 중단된 트랜잭션으로 인한 데이터 불일치 발생.
    *   **해결:** **ShedLock** 기반의 보정 스케줄러를 구현하여, 인프라 복잡도는 낮추면서도 중단된 사가(Saga)를 자동으로 복구하도록 설계.
    <details>
    <summary>[스케줄러 로직 흐름도]</summary>

    ```mermaid
    flowchart TD
    subgraph Scheduler["Reconciliation Scheduler (5분 주기)"]
    direction TB
    
            subgraph Loop1 ["1. 멈춘 사가 처리"]
                Start1("사가 조회<br/>(Status: PROGRESS / FAILED)") --> IsCreate1{"주문 생성인가?"}
                IsCreate1 -- Yes --> ActionRollback1["보상 (Rollback)"]:::rollback
                IsCreate1 -- "No (취소/반품)" --> ActionRetry["재시도 (Retry)"]:::retry
            end
    
            subgraph Loop2 ["2. 도메인 미반영 사가 처리"]
                Start2("사가 조회<br/>(Status: COMPLETED + Unbridged)") --> IsCreate2{"주문 생성인가?"}
                IsCreate2 -- Yes --> ActionRollback2["보상 (Rollback)"]:::rollback
                IsCreate2 -- "No (취소/반품)" --> ActionBridge["동기화 (Bridge)"]:::bridge
            end
    
            subgraph Loop3 ["3. 미결제 장기 대기 주문 처리"]
                Start3("주문 조회<br/>(Status: PENDING + 1시간 경과)") --> FindSaga{"사가 존재 여부"}
                FindSaga -- "있음" --> ActionRollback3["보상 (Rollback)"]:::rollback
                FindSaga -- "없음 (유실)" --> ActionLog["처리 불가 -> 에러 로깅<br/>(Manual Check)"]:::alert
            end
            
            Loop1 ~~~ Loop2 ~~~ Loop3
        end
    
        classDef rollback fill:#f8d7da,stroke:#721c24,color:#721c24,stroke-width:2px;
        classDef retry fill:#fff3cd,stroke:#856404,color:#856404,stroke-width:2px;
        classDef bridge fill:#d1ecf1,stroke:#0c5460,color:#0c5460,stroke-width:2px;
        classDef safe fill:#d4edda,stroke:#155724,color:#155724,stroke-width:2px;
        classDef alert fill:#f8d7da,stroke:#721c24,color:#721c24,stroke-width:2px,stroke-dasharray: 5 5;
    ```
    </details>
3.  **장애 전파 차단**
    *   **문제:** HTTP 기반의 동기 통신(FeignClient) 환경에서 외부 서비스의 장애나 응답 지연 발생 시, 요청 스레드가 무한정 대기하며 스레드 풀(Thread Pool)이 고갈되어 주문 서비스 전체가 마비되는 연쇄 장애(Cascading Failure) 위험 노출.
    *   **해결:**
        1. **타임아웃(Timeout):** 연결(2초) 및 읽기(5초) 타임아웃을 짧게 설정하여 지연 발생 시 즉각 리소스를 반환하도록 튜닝.
        2. **재시도(Retry):** 일시적인 네트워크 깜빡임(Transient Fault)은 멱등성이 보장되게 설계된 API를 최대 3회(최초 시도 1회 + 재시도 2회) 재시도하여 가용성 확보.
        3. **서킷 브레이커(Circuit Breaker):** 반복적인 실패나 지연 감지 시 즉시 요청을 차단(Open)하고 빠른 실패(Fail-Fast)를 반환하여 장애 전파로부터 시스템을 보호.
    <details>
    <summary>[Retry와 Circuit Breaker 흐름]</summary>

    ```mermaid
    sequenceDiagram
    participant Client
    participant Service as 주문 서비스
    participant External as 외부 서비스
    
        Client->>Service: 요청
        Service->>External: 1차 시도 (실패)
        Note over Service: Retry 대기 (1s)
        Service->>External: 2차 시도 (실패)
        Note over Service: Retry 대기 (1s)
        Service->>External: 3차 시도 (실패)
        
        Note over Service: 3회 모두 실패 -> Circuit Breaker에 '1회 실패' 기록
        Service-->>Client: Fallback 응답
        
        Note over Service: 실패 누적으로 임계치 초과 시 -> 서킷 OPEN
    ```
    </details>    

#### 문서
*   [분산 트랜잭션과 Saga Pattern 설계](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Saga-Pattern.md)
*   [데이터 정합성 복구를 위한 스케줄링 전략](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Scheduling.md)
*   [장애 전파 차단과 Resilience4j 설정 전략](https://github.com/nhnacademy-be12-trillion/order/blob/dev/docs/wiki/Resilience4j.md)


---

## Contact
- **E-mail**: [Splleat@gmail.com](mailto:Splleat@gmail.com)

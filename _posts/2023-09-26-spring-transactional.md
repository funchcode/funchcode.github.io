---
layout: post
title: "[Spring] Transactional 정리"
subtitle: Spring의 Transactional 애노테이션 관련 내용을 정리한다.
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [db, spring, jpa, transaction]
---

### Spring Framework에서 설정할 때 주의할 점

1. 적절한 범위 설정
2. 예외에 대한 처리
3. PlatformTransactionManager 트랜잭션 관리자 구성
4. 경계 정의
5. 읽기 전용 트랜잭션
6. 적절한 격리 수준 isolation 설정
7. 전파 구성 주의 propagation
8. 중첩 트랜잭션 주의 nested
9. Spring AOP 사용 시 주의
10. Spring 트랜잭션 관리자, 외부 트랜잭션 관리자
11. Auto Commit Mode 주의
12. 데드락

---

### DB 트랜잭션을 설정하는 이유

1. 데이터 일관성 보장
2. 원자성 보장
3. 동시성 관리
4. 롤백 및 회복 관리
5. 무결성
6. 오류 처리 및 로깅

---

### 네가지 격리(isolation) 수준

1. READ UNCOMMITTED
- dirty read  
- 실제로는 잘 사용되지 않는다.

2. READ COMMITTED
3. REPEATABLE READ
4. SERIALIZABLE
- 동시성을 완전히 차단

---

### 락(Lock) 기반 동시성 제어 유형

1. 행 레벨 락 (Row-Level Locks)
2. 테이블 락 (Table-Level Locks)

---

### 데드락(Dead Lock)이 발생할 수 있는 상황

1. 상호 배제 (Mutual Exclusion)
2. 보유 및 대기 (Hold and Wait)
3. 비선점 (No Preemption)
4. 순환 대기 (Circular Wait)

---

### 트랜잭션 Propagation 구성 주의해야 할 점

1. 트랜잭션 메소드 경계 파악
- 메소드가 새로운 트랜잭션을 시작하는지 기존 트랜잭션에 참여하는지 파악

2. 적절한 트랜잭션 전파 옵션 사용
3. 트랜잭션 범위 최소화, 성능 향상 고려와 데이터 일관성 유지에 주의
4. 중첩 트랜잭션 주의
- 특정 상황에서만 유용, 불필요한 경우 코드 복잡도 증가

5. 트랜잭션 전파와 예외 처리 고려
- 예외가 발생할 때 각 트랜잭션 전파 옵션에 따라 롤백되거나 커밋 됨

6. 테스트와 검증
- 전파를 구성한 다음에는 트랜잭션 동작을 검증하는 테스트 코드를 작성

7. 트랜잭션 범위와 데이터베이스 커넥션 관리
- 데이터베이스 커넥션을 공유해서 사용할 수 있음

---

### 트랜잭션 경계를 나누는 기준

1. 단위 업무 Use Case
2. 논리적 연산의 완전성
3. 예외 처리 및 롤백
- 특정 작업이 실패한 경우 롤백해야 하는 범위와 커밋해야 하는 범위
4. 성능 최적화
- 데이터베이스 락 충돌을 줄이고 동시성을 향상시킬 목적
5. 복구 가능성 및 로깅

---

### Spring Framework @Transactional readonly 옵션

- 수정 작업을 시도하는 경우 예외가 발생
- 한 번 읽은 값은 캐시되어 트랜잭션 내에서 중복 읽기를 최소화할 수 있다.

---

### 중첩 트랜잭션(Nested Transaction)에서 주의할 점

1. 부모 트랜잭션 혼동으로 인해 원치 않는 롤백 발생
2. 부모 트랜잭션과 별개로 커밋, 롤백이 될 수 있어 데드락 위험성
3. 트랜잭션 간의 관계를 이해하고 있어야 하므로 코드 복잡성 증가
4. 테스트 어려움
5. 중첩 트랜잭션 지원은 DB 벤더마다 다를 수 있음

---

### Spring Framework @Transactional 의 propagation 옵션 설명

1. REQUIRED
- 새로운 트랜잭션 시작, 이미 실행 중인 트랜잭션이 있으면 참가 없으면 생성.
2. SUPPORTS
- 이미 실행 중인 트랜잭션이 있으면 참가 없으면 없는 대로.
3. MANDATORY
- 부모 트랜잭션이 없는 경우 예외 발생
4. REQUIRES_NEW
- 항상 새로운 트랜잭션이 시작, 독립적으로 커밋 또는 롤백 됨.
5. NOT_SUPPORTED
- 이미 실행 중인 트랜잭션이 있으면 취소하고 없이 진행.
6. NEVER
- 이미 실행 중인 트랜잭션이 있으면 예외. 트랜잭션 없이 진행되어야 함. 
7. NESTED
- 이미 실행 중인 트랜잭션이 있으면 해당 트랜잭션 내에 중첩된 트랜잭션을 시작.
- 중첩된 트랜잭션은 별도로 커밋 또는 롤백 될 수 있음.
- 부모 트랜잭션이 커밋되면 같이 커밋, 롤백되면 같이 롤백.
# Spring Batch JPA에서의 Write 성능 향상 전략

## 1. Merge vs Persist

[Spring Batch 4.2 버전에 도입](https://spring.io/blog/2019/09/17/spring-batch-4-2-0-rc1-is-released#faster-writes-with-the-code-jpaitemwriter-code) 되었습니다.

이 함수를 JpaItemWriter사용하여 javax.persistence.EntityManager#mergeJPA 지속성 컨텍스트에서 항목을 씁니다. 이는 항목의 지속적 상태를 알 수 없거나 업데이트 된 것으로 알려진 경우에 적합합니다. 그러나 데이터가 새로운 것으로 알려져 있고 삽입으로 간주되어야하는 많은 파일 처리 작업에서는 javax.persistence.EntityManager#merge효율적이지 않습니다.

이 릴리스에서는 이러한 시나리오 보다는 JpaItemWriter사용 persist하기 위해 새로운 옵션을 도입했습니다 merge. 이 새로운 옵션을 사용하면 JpaItemWriter벤치 마크 jpa-writer-benchmark 에 따라 데이터베이스에 백만 개의 항목을 삽입 하는 데 사용하는 파일 수집 작업 이 2 배 빠릅니다 .

연관된 객체가 proxy면 proxy객체를 확인하려고 select 쿼리를 조회한다.

이 클래스는 merge 대신 persist를 호출한다. 따라서 항상 새로운 객체를 저장할 때만 사용해야 한다.
저장할 엔티티가 영속성 컨텍스트에 있다. -> dirty checking
저장할 엔티티가 영속성 컨텍스트에 없다. -> persist 호출

## 2. JPQL vs SQL



## 3. 최종 비교
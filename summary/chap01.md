# chap01 JPA 소개

## 1.1 SQL을 직접 다룰 때 발생하는 문제점
1. 반복: 객체를 데이터베이스에 CURD하려면 너무 많은 SQL과 JDBC API를 코드로 작성해야 한다는 점이다. 
2. SQL 의존적인 개발: 데이터 접근 계층을 사용해서 SQL을 숨겨도 실제 실행되는 SQL을 확인해야 하기 때문에 
진정한 의미의 계층 분할이 어렵다.

> JDBC (Java Database Connectivity)
> JDBC is an application programming interface for the programing language Java,
> which defines how a client may access a database. It is a Java-based data access technology 
>used for Java database connectivity. It is part of the Java Standard Edition platform, from Oracle Corporation.

## 1.2 패러다임의 불일치  
- 객체와 관계형 데이터베이스의 패러다임 불일치  
    -> 객체 구조를 테이블 구조에 저장할 때 한계  
    -> 개발자가 이를 해결하기 위해 많은 시간과 코드가 필요한 것이 문제  
    
1. 상속: 
    - 객체는 상속이 있는데 테이블은 없다.  
2. 연관관계   
    - 객체: 참조를 사용해서 연관된 객체를 조회
    - 테이블: 외래키를 사용해서 조인을 사용해서 연관된 테이블을 조회 
3. 객체 그래프 탐색
    - SQL을 직접 다루면 처음 실행하는 SQL에 따라 객체 그래프를 어디까지 탐색할 수 있는지 정해진다.  
4. 비교
    - 객체 비교
        - 동일성 비교 == 객체 인스턴스의 주소 값 비교
        - 동등성 비교 equals 객체 내부의 값 비교
    - JPA 비교: 같은 트랜잭션일 때 같은 객체가 조회되는 것을 보장  
    
## 1.3 JPA란 무엇인가?
- JPA(Java Persistence API)는 자바 진영의 ORM 표준 기술이다. 애플리케이션과 JDBC 사이에서 동작한다.  
<img width="393" alt="jpa1" src="https://user-images.githubusercontent.com/45681372/123535982-9dd9de00-d762-11eb-94aa-f5bb5f29ab9c.png">

- ORM(Object-Relation Mapping) 프레임워크: 객체와 관계형 데이터베이스를 매핑해서 패러다임 불일치 문제를 대신 해결  

1. JPA 소개  
- 하이버네이트라는 오픈소스 ORM을 기반으로 만들어진 자바 ORM 기술 표준  
- 자바 ORM 기술에 대한 API 표준 명세로, JPA를 사용하려면 JPA를 구현한 ORM 프레임워크를 선택해야 한다.  
- 현재 JPA 2.1을 구현한 ORM 프레임워크는 하이버네이트, EclipseLink, DataNucleus가 있는데 하이버네이트가 가장 대중적 

2. 왜 JPA를 사용해야 하는가?
- 생산성: 패러다임 불일치를 위한 코드, CRUD용 SQL을 대신 작성해준다.  
- 유지보수: SQL을 직접 다루면 등록, 수정, 조회 SQL과 결과 매핑을 위한 JDBC API를 모두 수정해야하는데 대신 해준다.  
- 패러다임 불일치 해결: 상속, 연관관계, 객체 그래프 탐색, 비교하기 등 패러다임 불일치 문제를 해결해준다.  
- 성능: 애플리케이션과 데이터베이스 사이에서 다양한 성능 최적화 기회를 제공  
- 데이터 접근 추상화와 벤더 독립성: 애플리케이션과 데이터베이스 사이에 추상화된 데이터 접근 계층을 제공해서 특정 데이터베이스 기술에 종속되지 않도록 한다.  





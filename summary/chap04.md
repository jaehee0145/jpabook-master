# chap04 엔티티 매핑  

## 4.1 @Entity 
- JPA를 사용해서 테이블과 매핑할 클래스는 필수로 @Entity 붙여야 한다.  
- 기본 생성자는 필수
- final 클래스, enum, innterface, inner 클래스에는 사용할 수 없다.
- 저장할 필드에 final을 사용하면 안된다.  

## 4.2 @Table  
- 엔티티와 매핑할 테이블을 지정한다. 생략하면 매핑한 엔티티 이름을 테이블 이름으로 사용한다.  

## 4.4 데이터베이스 스키마 자동 생성  
- JPA는 데이터베이스스키마를 자동으로 생성하는 기능을 지원  
```java
<property name="hibernate.hbm2ddl.auto" value="create" />
```
- 속성
    - create: 기존 테이블을 삭제하고 새로 생성 (DROP + CREATE)
    - create-drop: 종료할때 생성한 DDL제거 (DROP + CREATE + DROP)
    - update: 디비 테이블과 엔티티 매핑정보를 비교해서 변경 사항만 수정  
    - validate: 차이가 있으면 경고를 남기고 애플리케이션을 실행 안함
    - none: 자동 생성 기능을 사용하지 않으려면 유효하지 않은 옵션 값을 주면 된다.  
    
## 4.6 기본 키 매핑
- JPA가 제공하는 데이터베이스 기본 키 생성 전략은 다음과 같다.  
    - 직접 할당: 기본 키를 애플리케이션에 직접 할당
    - 자동 생성: 대리 키 사용 방식
        - 데이터베이스 벤더마다 지원하는 방식이 달라서 다양함
        
1. 기본 키 직접 할당 전략
- @Id로 매핑
- em.persist()로 엔티티를 저장하기 전에 애플리케이션에서 기본 키를 직접 할당 
```
Board board = new Board();
board.setId("id1"); // 기본 키 직접 할당 
em.persist(board);
``` 

2. IDENTITY 전략 
- 기본 키 생성을 데이터베이스에 위임하는 전략
- 주로 MySQL, PostgreSQL, SQL Server, DB2 에서 사용  
- ex) MySQL AUTO_INCREMENT : 데이터베이스가 기본 키를 자동으로 생성 
- 데이터베이스에 값을 저장하고 나서야 기본 키 값을 구할 수 있을때 사용한다. 
- @GeneratedValue(strategy = GenerationType.IDENTITY)
- 엔티티가 영속 상태가 되려면 식별자가 필요하기 때문에 이 전략은 트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.

3. SEQUENCE 전략 
- 데이터베이스 시퀀스는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트다.  
```sql
CREATE TABLE BOARD (
    ID BIGINT NOT NULL PRIMARY KEY,
    DATA VARCHAR(255)
)
// 시퀀스 생성
CREATE SEQUENCE BOARD_SEQ START WITH 1 INCREMENT BY 1;
```  
- em.persist()를 호출할 때 먼저 데이터베이스 시퀀스를 사용해서 식별자를 조회  
-> 조회한 식별자를 엔티티에 하랑한 후 영속성 컨텍스트에 저장  
-> 커밋하면 엔티티를 데이터베이스에 저장  

4. TABLE 전략
- 키 생성 전용 테이블을 하나 만들고 데이터베이스 시퀀스를 흉내내는 전략  
- 테이블을 사용하므로 모든 데이터베이스에 적용할 수 있다. 

5. AUTO 전략  
- GenerationType.AUTO는 선택한 데이터베이스 방언에 따라 IDENTITY, SEQUENCE, TABLE 전략 중 하나를 자동으로 선택
- 데이터베이스르 변경해도 코드를 수정할 필요가 없다.  


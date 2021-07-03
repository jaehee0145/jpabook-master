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
    
    
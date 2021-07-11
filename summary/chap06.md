# Chap06 다양한 연관관계 매핑
- 엔티티의 연관관계를 매핑할 때는 다음 3가지 고려해야 한다. 
1. 다중성 
    - 다대일 (@ManyToOne)
    - 일대다 (@OneToMany)
    - 일대일 (@OneToOne)
    - 다대다 (@ManyToMany)
    - 보통 다대일과 일대다 관계를 가장 많이 사용하고 다대다는 거의 사용 안함

2. 단방향, 양방향 
    - 테이블: 외래키 하나로 조인을 사용해서 양방향 쿼리가 가능하므로 방향 개념이 없음
    - 객체: 참조용 필드를 가지고 있는 객체만 연관된 객체를 조회할 수 있음
            한쪽만 참조하는 것을 단방향, 양쪽이 서로 참조하는 것을 양방향이라고 함
3. 연관관계의 주인
    - 테이블
        - 외래키 하나로 두 테이블이 연관관계를 맺는다. 
        - 테이블의 연관관계를 관리하는 포인트는 외래키 하나 
    - 엔티티
        - 양방향으로 매핑하면 연관관계 관리 포인트가 두 곳! 
        - 데이터베이스 외래키를 관리할 연관관계의 주인을 정해줘야 함  
        - 보통 외래키를 가진 테이블과 매핑한 엔티티를 주인으로 함  
        - 주인이 아닌 방향은 외래키를 변경할 수 없고 읽기만 가능  
        - 연관관계의 주인은 mappedBy 속성을 사용하지 않음  
        - 주인이 아니면 mappedBy 속성을 사용해서 연관관계의 주인필드 이름을 값으로 입력    
        
## 6.1 다대일 
- 다대일 <-> 일대다 : 항상 반대  
- 테이블 외래키는 항상 다쪽에 있음 -> 객체 양방향 관계에서 연관관게의 주인은 항상 다쪽  
1. 다대일 단방향 [N:1] 
2. 다대일 양방향 [N:1, 1:N]
<img width="688" alt="스크린샷 2021-07-11 오후 2 04 09" src="https://user-images.githubusercontent.com/52476758/125183352-e7d9be00-e250-11eb-957f-3d46a8d75be6.png">
    - 실선이 연관관계의 주
    - 양방향은 외래키가 있는 쪽이 연관관계의 주인이다. 
        ex. 회원, 팀이 있는 경우- 회원이 다N 쪽이고 팀의 외래키를 가지므로 연관관계의 주인!
            `@OneToMany(mappedBy="team")` 팀이 mappedBy 속성을 사용하고 주인 필드 이름을 값으로 입력해야함.
    - 양방향 연관관계는 항상 서로를 참조해야 한다. 
        - `setTeam` , `addMember` 같은 편의 메서드가 필요한데 양쪽 다 작성할때는 무한루프에 빠지지 않게 주의  
        
## 6.2 일대다  
- 엔티티를 하나 이상 참조할 있으므로 자바 컬렉션인 List, Set, Map, Collection 중 하나를 사용  
1. 일대다 단방향 [1:N]  
<img width="622" alt="스크린샷 2021-07-11 오후 2 05 08" src="https://user-images.githubusercontent.com/52476758/125183367-093aaa00-e251-11eb-96ce-ce8ddd4e4b03.png">
    - 특이한 관계  
        - 보통 자신이 매핑한 테이블의 외래키를 관리하는데 이 매핑은 반대쪽 테이블에 있는 외래키를 관리  
        - ex. Team이 List members를 참조하는데 (Team에서만 member를 호출할 수 있는데) member 테이블에서 team_id 외래키를 가지고 있음  
    - 다대일 양방향 매핑을 권장한다.  
    
2. 일대다 양방향 [1:N] 
- 일대다 양방향 매핑은 존재하지 않는다!
- 더 정확히 말하자면 양방향 매핑에서 일1이 연관관계 주인이 될 수 없다.  
- 될 수 있으면 다대일 양방향 매핑을 권장한다.  

## 6.3 일대일 [1:1] 
- 주 테이블, 대상 테이블 중에 누가 외래키를 가질지 선택해야 한다.  
1. 주 테이블에 외래키
    - 주 객체가 대상 객체를 참조하는 것과 유사한 구조
    - 객체지향 개발자들이 선호 
    - 장점은 주 테이블만 확인해도 대상 테이블과 연관관계가 있는지 알 수 있다. 
    1) 단방향  
    <img width="571" alt="스크린샷 2021-07-11 오후 4 35 47" src="https://user-images.githubusercontent.com/52476758/125186582-0d24f700-e266-11eb-9a6f-96ca774d4be4.png">
    - 디비에 LOCKER_ID 외래키에 유니크 제약 조건(UNI) 추가  
    2) 양방향 
    <img width="562" alt="스크린샷 2021-07-11 오후 4 40 27" src="https://user-images.githubusercontent.com/52476758/125186676-b4a22980-e266-11eb-8f07-63bb90107516.png">
    - 양방향이므로 연관관계의 주인을 정해야 한다.  

2. 대상 테이블에 외래키 
    - 단방향은 JPA에서 지원하지 않는다. 
    - 양방향   
    <img width="571" alt="스크린샷 2021-07-11 오후 4 42 45" src="https://user-images.githubusercontent.com/52476758/125186735-06e34a80-e267-11eb-8899-f29423b16dbd.png">
    
    
## 6.4 다대다 [N:N]
- 관계형 디비
    - 정규환된 테이블 2개로 다대다 관계를 표현할 수 없다. 
    - 연결 테이블을 추가해서 다대다 관계를 일대다, 다대일 관계로 풀어낼 수 있다.  
- 객체 
    - 테이블과 다르게 객체2개로 다대다 관계를 만들 수 있다. 
    - @ManyToMany  
1. 다대다: 단방향  
    - @JoinTable 을 사용해서 연결테이블을 바로 매핑 
    ```java
    @Entity
   public class Member {
       @Id
       @Column(name = "MEMBER_ID")
       private String id;
   
       private String username;
   
       @ManyToMany
       @JoinTable(name = "MEMBER_PRODUCT",     // 연결 테이블 지정
                   joinColumns = @JoinColumn(name = "MEMBER_ID"),  //현재 방향인 회원과 매핑할 컬럼
                   inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID")) // 반대 방향
       private List<Product> products = new ArrayList<>();       
   }  
   
2. 다대다: 양방향  
    - 역방향도 @ManyToMany 사용 
    - 원하는 곳에 mappedBy로 연관관계 주인 지정  
    - 연관관계 편의 메소드를 추가하는 것이 좋다 
    ```java
    public void addProduct(Product product) {
       ...
       products.add(product);
       product.getMembers.add(this);
   }

3. 다대다: 매핑의 한계와 극복, 연결 엔티티 사용  
- 실무에서는 연결 테이블에 추가로 칼럼이 필요하고 기존 엔티티에서 추가한 칼럼을 매핑할 수 없다.  
- 엔티티를 다대일, 일대다 관계로 풀어줄 연결 엔티티가 필요하다.  
- 연결 엔티티   
    - 복합 기본키 
        - 각 엔티티의 기본키로 이루어진 복합 기본키를 사용하려면 별도의 식별자 클래스를 만들어야 한다.  
        - 복잡한 방법
        
4. 다대다: 새로운 기본키 사용  
    - 추천하는 기본키 생성 전략은 디비에서 생성해주는 대리키를 Long값으로 사용
        - 간편하고 영구히 쓸수 있고 비즈니스에 의존적이지 않다. 
        - ORM 매핑시 복합 키를 만들지 않아도 되므로 간단   
5. 다대다 연관관계 정리  
    - 일대일, 다대일 관계로 풀기 위해 식별자를 어떻게 구성할지 선택해야 한다.  
        - 식별관계: 받아온 식별자를 기본키 + 외래키로 사용 
        - 비식별관계: 받아온 식별자는 외래키로, 새로운 식별자 추가  
    - 비식별 관계를 추천  
  











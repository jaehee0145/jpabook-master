# chap05 연관관계 매핑 기초  
- 엔티티들은 대부분 다른 엔티티와 연관관계가 있다.  
- 객체관계 매핑ORM 에서 가장 어려운 부분이 바로 객체 연관관계와 테이블 연관관계를 매핑하는 일이다.  

## 5.1 단방향 연관관계  

<img width="717" alt="스크린샷 2021-07-06 오후 9 18 35" src="https://user-images.githubusercontent.com/45681372/124598723-e0f12b00-de9f-11eb-9250-477adb143f5d.png">  
  
1. 로직
    - 회원과 팀이 있다.
    - 회원은 하나의 팀에만 소속될 수 있다.
    - 회원과 팀은 다대일 관계다.  
    
2. 객체 연관관계   
    - Member.team 필드로 팀 객체와 연관관계
    - 회원 객체와 팀 객체는 단방향 관계
    - member.getTeam() 가능하지만 반대는 불가 
    
3. 테이블 연관관계
    - MEMBER에 TEAM_ID가 외래키로 있다.
    - 회원 테이블과 팀 테이블은 양방향 관계  
    ```sql
    SELECT * FROM MEMBER M JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID; // member join team
    SELECT * FROM TEAM T JOIN MEMBER M ON M.TEAM_ID = T.TEAM_ID; // team join team

- 객체 연관관계와 테이블 연관관계의 가장 큰 차이
    - 참조를 통한 연관관계는 언제나 단방향
        - 양방향 : 단방향 + 단방향
    - 테이블은 외래키 하나로 양방향 조인 가

- 객체 관계 매핑  
```java 
@Entity
public class Member {
    @Id
    @Column(name = "MEMBER_ID")
    private String id;
    
    private String username;

    //연관관계 매핑
    @ManyToOne                  // 이름 그대로 다대일 관계라는 매핑 정보
    @JoinColum(name="TEAM_ID")  // 외래 키를 매핑할때 사용, 생략 가능 
    private Team team;

    //연관관계 설정
    public void setTeam(Team team) {
        this.team = team;
    }
}
``` 

## 5.3 양방향 연관관계  
  <img width="769" alt="스크린샷 2021-07-06 오후 9 47 41" src="https://user-images.githubusercontent.com/45681372/124602427-cc169680-dea3-11eb-8705-eb2a9aec8e0d.png">

```java 
@Entity
public class Team {
    @Id
    @Column(name = "TEAM_ID")
    private String id;
    
    private String name;

    // 추가
    @OneToMAny(mappedBy = "team")
    private List<Member> members = new ArrayList<Member>();
}
``` 

## 5.4 연관관계의 주인  
- 양방향 연관관계 : 테이블은 외래 키 하나 vs 객체의 참조는 둘
- 연관관계의 주인: 두 객체 연관관계 중 테이블의 외래 키를 관리할 주체  
- 주인만 외래키를 관리할 수 있다.  
- 주인은 mappedBy 속성을 사용하지 않는다.  
- 주인이 아니면 mappedBy 속성을 사용해서 연관관계의 주인을 지정해야 한다.  





















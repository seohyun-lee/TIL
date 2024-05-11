## TIL - @JoinColumn과 mappedBy
평소 연관관계 매핑 시 사용하는 @JoinColumn과 mappedBy에 대해 알아보았다.

### 1. @JoinColumn - name과 referencedColumnName
@JoinColumn 애노테이션은 FK를 가지는 소유 측의 실제 물리적 매핑을 정의한다. @ManyToOne 애노테이션을 쓸 때 같이 사용하고는 했다.
- @JoinColumn(name): 매핑할 외래 키 컬럼명 (참조 대상을 이 테이블에 어떤 컬럼명으로 설정할 것인지) <br>
  기본값은 필드명_\[참조하는 테이블의 기본 키 컬럼명]
> ex. Member-Order의 일대다 양방향 관계에서 Order 클래스에 다음과 같이 Member 엔티티를 필드로 둘 때
  Order에 JoinColumn(name="memnber_id")을 설정 -> Order 테이블에 컬럼명 member_id
  ```
  @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name="member_id")
    private Member member;
  ```
- @JoinColumn(referencedColumnName): 외래 키가 참조하는 대상 테이블의 컬럼명을 지정. 생략 시 대상 테이블의 PK로 자동 지정된다. (PK가 아닌 다른 컬럼에 지정하는 것은 정규화 관점에서 권장하지 않음)
- 그외 파라미터들: foreignKey, unique, nullable, updatable
<br>
개발 시 @ManyToOne(name="") 애노테이션을 붙여야 한 이유를 더 이해하게 되었다. 기본 컬럼명 설정값이 member_member_id여서.
> @JoinColumn을 지정하지 않으면 조인 테이블 전략을 활용한다는데 이와 관련해 더 알아봐야 할 것 같다.

### 2. mappedBy 옵션, @OneToMany와 @OneToOne에서
양방향 매핑을 할 때 참조 측에서 사용한다. 참조 측은 단순히 소유 측에 매핑된다. @OneToMany 애노테이션을 쓸 때 사용하고는 했다. <br>
양방향 관계에서 두 객체가 서로 연결되면 서로를 변경하려 할 수 있는 문제점이 있다. <br>
mappedBy는 한 테이블 중 하나의 테이블만 관리할 수 있도록 정하는 것이다. MappedBy가 정의되지 않은 객체가 연관관계의 주인이다.


## 레퍼런스
- https://www.baeldung.com/jpa-join-column
- https://www.baeldung.com/jpa-joincolumn-vs-mappedby

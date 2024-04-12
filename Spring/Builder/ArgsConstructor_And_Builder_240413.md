## TIL - ArgsConstructor And Builder
---
이전에 Dto 클래스를 작성할 때 클래스 레벨에 @Builder, @NoArgsConstructor, @AllArgsConstructor를 무분별하게 붙여서 사용했는데, 이해가 부족해서 그렇다는 생각이 들어서 이에 대해서 공부해 보았다.

### @NoArgsConstructor(AccessLevel.PROTECTED) 사용 이유
* 기본 생성자는 지연 로딩을 하기 위해 필요하다. JPA의 지연 로딩에는 실제 객체를 상속한 프록시 객체가 사용되는데 이를 위해 super()을 호출한다. 따라서 실제 객체의 기본 생성자가 필요하다.
* AccessLevel.PROTECTED로 접근을 제한하는 이유: 무분별한 사용을 막기 위해. AccessLevel.PRIVATE이면 super()를 호출할 수 없다.

### 클래스 레벨에 @Builder 사용 시 @AllArgsConstructor 붙이는 이유
* @Builder를 붙인 클래스에 생성자가 없다면 @Builder는 @AllArgsConstructor(access = AccessLevel.PACKAGE) 생성자를 만든다.
* 그러나 @ArgsConstructor를 붙여 기본생성자를 만들면 Builder는 자동으로 생성자를 만들지 않는다. 전체 필드를 받는 생성자가 필요하다면 이 어노테이션을 붙여야 한다. 

### 어떻게 작성해야 좋을까?
우선 클래스 레벨에 @NoArgsConstructor(AccessLevel.PROTECTED)를 쓴다.
그 후 (1), (2) 방법 중 택한다.
(1) 클래스 레벨에 @Builder, @AllArgsConstructor <br>
(2) 메서드 레벨에서 @Builder 붙여서 생성자 커스터마이징 (ex. 모든 argument라면 AllArgsConstructo와 같은 효과) <br>

## 레퍼런스
---
https://projectlombok.org/features/ <br>
https://www.baeldung.com/intro-to-project-lombok <br>
https://projectlombok.org/features/Builder <br>

## QnA
---
(스터디 이후에 추가 예정)

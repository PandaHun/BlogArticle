# VO, DTO, Entity

우아한 테크 세미나의 우아한 CRUD를 보고 공부한 내용입니다.

## 서론

종종 VO(Value Object), DTO(Data Transfer Object), Entity를 혼용해서 사용하면서 혼돈을 일으킨다.

그렇기 때문에 제대로 알아보려고 합니다.

## 본론

### VO(Value Object)

* 값 객체
* 값이 같으면 같은 객체라고 인식하는 식별성이 없는 작은 객체
  * ex) Money, Color 등
* 핵심적인 역할로는 `equals()`와 `hashcode()` 를 오버라이딩 한다.
* 또한 테이블 속성 외의 추가적인 속성을 가질 수 있다.
* ReadOnly

### DTO(Data Transfer Object)

* 데이터 전송 객체
* 네트워크 전송 시 Data holder의 느낌이 강하다.
* 즉 데이터 가공 처리를 위해 감싸주는 객체.

### Entity

* JPA `@Entity`로 익숙한 개념.
* 테이블과 대응되는 객체로 테이블과 1:1 매칭이 된다.
* DB 테이블의 컬럼만을 속성으로 가진다.
* VO와 다르게 식별성을 가지고 있다.

## 결론

어떤것이 맞다고 할 수 없으나, 속한 팀이나 회사에서 규칙을 정하고 작성하는게 편하다.
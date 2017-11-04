# 매핑 관계에서 실제 쿼리 호출 시점

## getId()

* 매핑 클래스의 인스턴스를 호출하는 시점에는 프록시를 초기화하지 않는다.
* 실제 해당 인스턴스의 **필드에 직접 접근할 때** 프록시를 초기화하여 쿼리가 호출된다.
* getId()의 경우 @Access 설정에 따라 다르다.
	* JPA가 엔티티 데이터에 접근하는 방식을 말한다.
	* 사실 @Id의 위치에 따라 동작하므로 @Access 애너테이션은 생략해도 된다.

### @Access(AccessType.FIELD)

* 필드에 직접 접근하는 설정이다. @Id의 위치를 필드에 직접 설정한 것과 같다.
* JPA는 getId() 메소드가 id만 가져오는 메소드인지, 다른 필드까지 활용해서 어떤 일을 하는 메소드인지 알지 못하므로 프록시 객체를 초기화한다.
* **즉, getId()만 호출하더라도 쿼리가 발생한다.**

```java
@Data
@Entity
@Access(AccessType.FIELD) // 생략 가능
public class Product {
	@Id
	@SequenceGenerator(name = "PRODUCT_SEQ_GENERATOR", sequenceName = "PRODUCT_SEQ", allocationSize = 2)
	@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "PRODUCT_SEQ_GENERATOR")
	private Long id;

	private String name;

	@ManyToOne(fetch = FetchType.LAZY, optional = false)
	private Seller seller;
}
```

### @Access(AccessType.PROPERTY)

* 접근자를 사용하는 설정이다. getter에 설정한 것과 같다.
* JPA는 getId() 메소드가 식별자임을 알고 있으므로 프록시를 초기화하지 않고도 식별자를 리턴해줄 수 있다.
* **즉, getId() 호출에 의해 쿼리가 발생하지 않는다.**
* 물론 다른 필드에 접근하면 프록시를 초기화하여 쿼리가 발생한다.

```java
@Data
@Entity
@Access(AccessType.PROPERTY) // 생략 가능
public class Product {
	private Long id;
	private String name;
	private Seller seller;

	@Id
	@SequenceGenerator(name = "PRODUCT_SEQ_GENERATOR", sequenceName = "PRODUCT_SEQ", allocationSize = 2)
	@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "PRODUCT_SEQ_GENERATOR")
	public Long getId() {
		return id;
	}

	@ManyToOne(fetch = FetchType.LAZY, optional = false)
	public Seller getSeller() {
		return seller;
	}
}
```

## 참고

* <https://stackoverflow.com/questions/3941797/how-can-i-retrieve-the-foreign-key-from-a-jpa-manytoone-mapping-without-hitting>

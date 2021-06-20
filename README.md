# spring_framework_study
## 인프런 강의노트
- [자바 스프링 프레임워크(renew ver.) - 신입 프로그래머를 위한 강좌](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC_renew)

### pom.xml

1. pom.xml을 통해 필요한 라이브러리만 다운해서 사용한다.
2. 프로젝트에 필요한 모듈 가져요기

### 자바 프로젝트와 스프링의 차이점

1. 스프링 컨테이너 안에 bin 객체를 생성하면 객체를 따로 생성하지 않아도 된다.
> 자바 객체 생성
```java
TranspotationWalk transpotationWalk = new TranspotationWalk();
transpotationWalk.move();
```
> 스프링 객체 생성
```java
GenericXmlApplicationContext ctx = new GenericXmlApplicationContext("classpath:applicationContext.xml");
		
TranspotationWalk transpotationWalk = ctx.getBean("tWalk", TranspotationWalk.class);
transpotationWalk.move();
		
ctx.close();
```

2. 스프링 컨테이너란 bin 객체 메모리가 로드되는 곳을 말한다.
3. 스프링에서 객체 생성은 컨테이너가 대신 해준다.
4. 요즘엔 Xml 방식보단 어노테이션을 많이 이용한다.

### 스프링 설정 파일 분리
하나의 스프링 설정파일에 모든 bean 객체를 저장하면 관리가 어렵다.
applicationContext.xml 를 appCtx1.xml, appCtx2.xml, appCtx3.xml 과 같이 기능별로 분리하여 저장한다.
bean 객체를 사용할 때엔 String 배열을 사용한다.
```java
String[] appCtxs = {"classpath:appCtx1.xml", "classpath:appCtx2.xml", "classpath:appCtx3.xml"};
GenericXmlApplicationContext ctx = new GenericXmlApplicationContext(appCtxs);
```

### 빈(Bean)의 범위(Scope)
bean 객체는 싱글톤과 프로토타입이 있다.
싱글톤은 모두 같은 인스턴스를, 프로토타입의 경우 모두 다른 인스턴스를 생성한다.

### 의존객체 자동 주입  
의존객체를 자동으로 주입하면 <bean></bean> 태그 안의 <constructor-arg>를 사용하지 않아도 된다. 
의존객체 자동 주입 방식에는 @Autowired 방식과 @Resource 방식이 있다.

| 구분 | @Autowired 방식 | @Resource 방식                                                                                         |
| :----- | ---------------------------------------- | ---------------------------------------- |
| 의존객체를 찾는 방법 | 객체타입 일치 | 객체이름 일치 |
| 사용 | 생성자, 프로퍼티, 메소드 모두 사용 가능 | 프로퍼티, 메소드만 사용 가능/생성자X |  
  
@Resource 방식의 경우 생성자에 사용할 수 없기 때문에 default 생성자를 꼭 명시해주어야 한다. (@Autowired를 프로퍼티나 메소드에만 사용했을 경우에도 마찬가지이다.)

### 의존객체 선택
* 동일한 타입의 bean 객체가 여러개라면 스프링은 어떤 bean 객체를 선택해야할까?
	
```java
<bean id="wordDao1" class="com.word.dao.WordDao" />
<bean id="wordDao2" class="com.word.dao.WordDao" />
<bean id="wordDao3" class="com.word.dao.WordDao" />
```
이 경우 스프링은 어떤 bean 객체를 선택할지 몰라 Exception을 발생시킨다.
> 해결책
```java
<bean id="wordDao1" class="com.word.dao.WordDao" >
    <qualifier value="usedDao"/>
</bean>
<bean id="wordDao2" class="com.word.dao.WordDao" />
<bean id="wordDao3" class="com.word.dao.WordDao" />
```
	
> @Autowired와 거의 같은 기능을 하는 방식에는 @Inject 가 있다. 차이점은 @Inject는 @Qualifier 대신 @Named 를 사용한다.

### Bean 객체 생명주기
> 빈 객체가 생성되고 소멸될 때 특정한 작업을 자동으로 진행하고 싶을 때 다음 방법을 이용한다.
1. interface 이용
```java
public class BookSearchService implements InitializingBean, DisposableBean {
	//중간부분 생략
	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("object creat");
	}
	
	@Override
	public void destroy() throws Exception {
		System.out.println("object dispose");		
	}
	
}
```
2. init-method, destroy-method 이용
```
<bean id="memberRegisterService" class="com.brms.member.service.MemberRegisterService"
	init-method="initMethod" destroy-method="destroyMethod"
/>	
```

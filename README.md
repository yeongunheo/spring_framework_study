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

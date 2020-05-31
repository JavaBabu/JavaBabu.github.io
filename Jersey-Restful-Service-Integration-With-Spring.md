# Jersey Restful Service Integration With Spring

:-Vikas   [http://www.javababu.com](http://www.javababu.com/)

This blog shows how to integrate Jersey Restful Service with Spring Framework. Jersey has provided SpringServlet class using which we can make a spring bean exposed as Restful Service by annotating with Jax-rs annotations.

[Jersey](https://jersey.java.net/) is an open source framework for developing RESTful Web Services. It serves as a reference implementation of JAX-RS.

**Followings technologies are used** :

1. Jersy 1.9
2. Spring 4.x
3. Maven
4. Java 1.8
5. STS

**Followings are project Structures :**



![](https://github.com/JavaBabu/JavaBabu.github.io/blob/master/spring-jersy-project-structure.PNG)

**Followings are dependencies:**
```xml
<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
		</dependency>
		<dependency>
			<groupId>javax.xml.bind</groupId>
			<artifactId>jaxb-api</artifactId>
			<version>2.3.0</version>
			<classifier>sources</classifier>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>com.sun.jersey.contribs</groupId>
			<artifactId>jersey-spring</artifactId>
			<version>1.19.3</version>
			<exclusions>
				<exclusion>
					<groupId>org.springframework</groupId>
				</exclusion>
				<exclusion>
					<groupId>org.springframework</groupId>
					<artifactId>spring-core</artifactId>
				</exclusion>
				<exclusion>
					<groupId>org.springframework</groupId>
					<artifactId>spring-web</artifactId>
				</exclusion>
				<exclusion>
					<groupId>org.springframework</groupId>
					<artifactId>spring-beans</artifactId>
				</exclusion>
				<exclusion>
					<groupId>org.springframework</groupId>
					<artifactId>spring-context</artifactId>
				</exclusion>
				<exclusion>
					<groupId>org.springframework</groupId>
					<artifactId>spring-aop</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.3.8.RELEASE</version>
		</dependency>
	</dependencies>

```

We exposed Spring bean as Restful Service, so that  we can use ioc container like dependency injection.
```java
@Component
@Path("/transaction")
public class Transaction {

	@Autowired
	protected TransactionService transactionService;

	@GET
	@Produces(MediaType.APPLICATION_XML)
	@Path("/payments/{payId}")
	public List<EPayment> getTransactions(@PathParam("payId") String payId) {
		List<EPayment> ePayments = null;
		ePayments = transactionService.transactionDetails(payId);
		return ePayments;
	}
}


```

Here Transaction is spring Bean class annotated with @Component annotation. For using as Rest Endpoint, we need to register with Jersy runtime, this can be done by SpringServlet provided by Jersy.

```xml
<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/application-context.xml</param-value>
	</context-param>
	<servlet>
		<servlet-name>jersey-servlet</servlet-name>
		<servlet-class>com.sun.jersey.spi.spring.container.servlet.SpringServlet</servlet-class>
		<init-param>
			<param-name>com.sun.jersey.config.property.packages</param-name>
			<param-value>com.jb.resource</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>jersey-servlet</servlet-name>
		<url-pattern>/api/*</url-pattern>
	</servlet-mapping>
```

_application-context.xml_ – Register bean and enable the component auto scanning feature.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

	<context:component-scan
		base-package="com.jb.service, com.jb.resource" />
</beans>

```
Here it’s service class
```java
@Service
public class TransactionService {


	public List<EPayment> transactionDetails(String payId) {
		List<EPayment> payments = null;
		payments=new ArrayList<>();
		payments.add(new EPayment("1000040527856", "Vikas", 15000));
		return payments;

	}
}
```

Here it’s dto class
```java
@XmlRootElement(name = "e-pay")
@XmlType
@XmlAccessorType(XmlAccessType.FIELD)
public class EPayment {
	@XmlElement(name = "acc-no")
	protected String accNo;
	@XmlElement(name = "name")
	protected String Name;
	protected float amount;

	public EPayment() {
		
	}
	public EPayment(String accNo, String name, float amount) {
		super();
		this.accNo = accNo;
		Name = name;
		this.amount = amount;
	}

	public String getAccNo() {
		return accNo;
	}

	public void setAccNo(String accNo) {
		this.accNo = accNo;
	}

	public String getName() {
		return Name;
	}

	public void setName(String name) {
		Name = name;
	}

	public float getAmount() {
		return amount;
	}

	public void setAmount(float amount) {
		this.amount = amount;
	}

}

```

Url:-[http://localhost:8181/jersy-test/api/transaction/payments/111](http://localhost:8181/jersy-test/api/transaction/payments/111)

![](https://github.com/JavaBabu/JavaBabu.github.io/blob/master/spring-jersy-output.PNG)

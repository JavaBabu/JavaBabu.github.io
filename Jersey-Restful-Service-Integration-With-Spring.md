# Jersey Restful Service Integration With Spring

:-Vikas[http://www.javababu.com](http://www.javababu.com/)

This blog show how to integrate Jersey Restful Service with Spring Framework. Jersey has provided SpringServlet class using which we can make a spring bean exposed as Restful Service by annotating with Jax-rs annotations.

[Jersey](https://jersey.java.net/) is an open source framework for developing RESTful Web Services. It serves as a reference implementation of JAX-RS.

**Followings technologies are used** :

1. Jersy 1.9
2. Spring 4.x
3. Maven
4. Java 1.8
5. STS

**Followings are project Structures :**

![](RackMultipart20200524-4-7kje9h_html_aa505494bda27137.png)

**Followings are dependencies:**

\&lt;dependencies\&gt;

\&lt;dependency\&gt;

\&lt;groupId\&gt;junit\&lt;/groupId\&gt;

\&lt;artifactId\&gt;junit\&lt;/artifactId\&gt;

\&lt;version\&gt;3.8.1\&lt;/version\&gt;

\&lt;scope\&gt;test\&lt;/scope\&gt;

\&lt;/dependency\&gt;

\&lt;dependency\&gt;

\&lt;groupId\&gt;javax.servlet\&lt;/groupId\&gt;

\&lt;artifactId\&gt;javax.servlet-api\&lt;/artifactId\&gt;

\&lt;version\&gt;3.1.0\&lt;/version\&gt;

\&lt;/dependency\&gt;

\&lt;dependency\&gt;

\&lt;groupId\&gt;javax.xml.bind\&lt;/groupId\&gt;

\&lt;artifactId\&gt;jaxb-api\&lt;/artifactId\&gt;

\&lt;version\&gt;2.3.0\&lt;/version\&gt;

\&lt;classifier\&gt;sources\&lt;/classifier\&gt;

\&lt;scope\&gt;provided\&lt;/scope\&gt;

\&lt;/dependency\&gt;

\&lt;dependency\&gt;

\&lt;groupId\&gt;com.sun.jersey.contribs\&lt;/groupId\&gt;

\&lt;artifactId\&gt;jersey-spring\&lt;/artifactId\&gt;

\&lt;version\&gt;1.19.3\&lt;/version\&gt;

\&lt;exclusions\&gt;

\&lt;exclusion\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;/exclusion\&gt;

\&lt;exclusion\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;artifactId\&gt;spring-core\&lt;/artifactId\&gt;

\&lt;/exclusion\&gt;

\&lt;exclusion\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;artifactId\&gt;spring-web\&lt;/artifactId\&gt;

\&lt;/exclusion\&gt;

\&lt;exclusion\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;artifactId\&gt;spring-beans\&lt;/artifactId\&gt;

\&lt;/exclusion\&gt;

\&lt;exclusion\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;artifactId\&gt;spring-context\&lt;/artifactId\&gt;

\&lt;/exclusion\&gt;

\&lt;exclusion\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;artifactId\&gt;spring-aop\&lt;/artifactId\&gt;

\&lt;/exclusion\&gt;

\&lt;/exclusions\&gt;

\&lt;/dependency\&gt;

\&lt;dependency\&gt;

\&lt;groupId\&gt;org.springframework\&lt;/groupId\&gt;

\&lt;artifactId\&gt;spring-webmvc\&lt;/artifactId\&gt;

\&lt;version\&gt;4.3.8.RELEASE\&lt;/version\&gt;

\&lt;/dependency\&gt;

\&lt;/dependencies\&gt;

We exposed Spring bean as Restful Service, so that we can use ioc container like dependency injection.

@Component

@Path(&quot;/transaction&quot;)

**public**** class** Transaction {

@Autowired

**protected** TransactionService transactionService;

@GET

@Produces(MediaType._ **APPLICATION\_XML** _)

@Path(&quot;/payments/{payId}&quot;)

**public** List\&lt;EPayment\&gt; getTransactions(@PathParam(&quot;payId&quot;) String payId) {

List\&lt;EPayment\&gt; ePayments = **null** ;

ePayments = transactionService.transactionDetails(payId);

**return** ePayments;

}

}

Here Transaction is spring Bean class annotated with @Component annotation. For using as Rest Endpoint, we need to register with Jersy runtime, this can be done by SpringServlet provided by Jersy.

\&lt;listener\&gt;

\&lt;listener-class\&gt;org.springframework.web.context.ContextLoaderListener\&lt;/listener-class\&gt;

\&lt;/listener\&gt;

\&lt;context-param\&gt;

\&lt;param-name\&gt;contextConfigLocation\&lt;/param-name\&gt;

\&lt;param-value\&gt;/WEB-INF/application-context.xml\&lt;/param-value\&gt;

\&lt;/context-param\&gt;

\&lt;servlet\&gt;

\&lt;servlet-name\&gt;jersey-servlet\&lt;/servlet-name\&gt;

\&lt;servlet-class\&gt;com.sun.jersey.spi.spring.container.servlet.SpringServlet\&lt;/servlet-class\&gt;

\&lt;init-param\&gt;

\&lt;param-name\&gt;com.sun.jersey.config.property.packages\&lt;/param-name\&gt;

\&lt;param-value\&gt;com.jb.resource\&lt;/param-value\&gt;

\&lt;/init-param\&gt;

\&lt;load-on-startup\&gt;1\&lt;/load-on-startup\&gt;

\&lt;/servlet\&gt;

\&lt;servlet-mapping\&gt;

\&lt;servlet-name\&gt;jersey-servlet\&lt;/servlet-name\&gt;

\&lt;url-pattern\&gt;/api/\*\&lt;/url-pattern\&gt;

\&lt;/servlet-mapping\&gt;

_application-context.xml_ – Register bean and enable the component auto scanning feature.

\&lt;?xmlversion=_&quot;1.0&quot;_encoding=_&quot;UTF-8&quot;_?\&gt;

\&lt;beansxmlns=_&quot;http://www.springframework.org/schema/beans&quot;_

xmlns:xsi=_&quot;http://www.w3.org/2001/XMLSchema-instance&quot;_

xmlns:context=_&quot;http://www.springframework.org/schema/context&quot;_

xsi:schemaLocation=_&quot;http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd_

_http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd&quot;_\&gt;

\&lt;context:component-scan

base-package=_&quot;com.jb.service, com.jb.resource&quot;_/\&gt;

\&lt;/beans\&gt;

**Here it&#39;s service class**

@Service

**public**** class** TransactionService {

**public** List\&lt;EPayment\&gt; transactionDetails(String payId) {

List\&lt;EPayment\&gt; payments = **null** ;

payments= **new** ArrayList\&lt;\&gt;();

payments.add( **new** EPayment(&quot;1000040527856&quot;, &quot;Vikas&quot;, 15000));

**return** payments;

}

}

**Here it&#39;s dto class**

@XmlRootElement(name = &quot;e-pay&quot;)

@XmlType

@XmlAccessorType(XmlAccessType._ **FIELD** _)

**public**** class** EPayment {

@XmlElement(name = &quot;acc-no&quot;)

**protected** String accNo;

@XmlElement(name = &quot;name&quot;)

**protected** String Name;

**protected**** float**amount;

**public** EPayment() {

}

**public** EPayment(String accNo, String name, **float** amount) {

**super** ();

**this**.accNo = accNo;

Name = name;

**this**.amount = amount;

}

**public** String getAccNo() {

**return** accNo;

}

**public**** void** setAccNo(String accNo) {

**this**.accNo = accNo;

}

**public** String getName() {

**return** Name;

}

**public**** void** setName(String name) {

Name = name;

}

**public**** float** getAmount() {

**return** amount;

}

**public**** void**setAmount(**float**amount) {

**this**.amount = amount;

}

}

Url:-[http://localhost:8181/jersy-test/api/transaction/payments/111](http://localhost:8181/jersy-test/api/transaction/payments/111)

![](RackMultipart20200524-4-7kje9h_html_81cddee9c4d44670.png)

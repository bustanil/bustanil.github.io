---
layout: post
title: Spring Boot - Solving 'missing EmbeddedServletContainerFactory bean'
tags: spring java
---

Spring Boot is a great addition to already complete Java enterprise solution. It helps developers by making it easier to setup and pack Spring-based applications.

I am trying to migrate one of my Spring application to utilize Spring Boot. This application uses Spring Remoting module to export services as RMI services. RMI does not need web server so I use `spring-boot-starter` as my starter.

     <dependency>
        <groupId>org.springframework.boot</groupId>
    	<artifactId>spring-boot-starter</artifactId>
    </dependency>

But when I try to start the application Spring Boot spits out errors:

	Caused by: org.springframework.context.ApplicationContextException: Unable to start EmbeddedWebApplicationContext due to missing EmbeddedServletContainerFactory bean.
	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.getEmbeddedServletContainerFactory(EmbeddedWebApplicationContext.java:185)
	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.createEmbeddedServletContainer(EmbeddedWebApplicationContext.java:158)
	at org.springframework.boot.context.embedded.EmbeddedWebApplicationContext.onRefresh(EmbeddedWebApplicationContext.java:132)
	... 12 more

It seems that Spring Boot thinks that my application is a web application. But my application uses only RMI and does not need a web container. I google the exception and people suggest that I should use `spring-boot-starter-web` as my starter. So I tried `spring-boot-starter-web` and it works. But that does not make sense. I do not want unnecessary dependencies to be included in my application.

I look into Spring Boot's source code and found out that it tries to deduce what kind of application is running by checking one or more classes in the classpath. For example, if `javax.servlet.Servlet` and `org.springframework.web.context.ConfigurableWebApplicationContext` class exist in the classpath then Spring Boot assumes that the application is a web application and tries to start an embedded web container.

My application uses Spring Remoting and it depends on `javax.servlet` and `spring-web` though I don't use them. To avoid my application being detected as a web application, I exclude both dependencies from my `pom.xml`

	<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-remoting</artifactId>
        <version>2.0.8</version>
        <exclusions>
            <exclusion>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
            </exclusion>
            <exclusion>
                <groupId>javax.servlet</groupId>
                <artifactId>servlet-api</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

And it runs perfectly now.

	  .   ____          _            __ _ _
	 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
	( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
	 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
	  '  |____| .__|_| |_|_| |_\__, | / / / /
	 =========|_|==============|___/=/_/_/_/
	 :: Spring Boot ::        (v1.0.2.RELEASE)

	2014-06-03 23:34:23.435  INFO 7796 - [main] AppServer : Starting AppServer on buzzuk with PID 7796 (C:\workspace\rmi_demo\target\classes started by Bustanil Arifin in C:\workspace\rmi_demo)
	2014-06-03 23:34:23.478  INFO 7796 - [main] AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@2210a531: startup date [Tue Jun 03 23:34:23 ICT 2014]; root of context hierarchy
	2014-06-03 23:34:24.147  INFO 7796 - [main] RmiServiceExporter : Looking for RMI registry at port '1199'
	2014-06-03 23:34:25.275  INFO 7796 - [main] RmiServiceExporter : Could not detect RMI registry - creating new one
	2014-06-03 23:34:25.286  INFO 7796 - [main] RmiServiceExporter : Binding service 'PersonService' to RMI registry: RegistryImpl[UnicastServerRef [liveRef: [endpoint:[10.195.93.94:1199](local),objID:[0:0:0, 0]]]]
	2014-06-03 23:34:25.467  INFO 7796 - [main] AnnotationMBeanExporter : Registering beans for JMX exposure on startup
	2014-06-03 23:34:25.487  INFO 7796 - [main] AppServer : Started AppServer in 2.407 seconds (JVM running for 2.769)


Sample source code can be found here: https://github.com/bustanil/rmi_demo
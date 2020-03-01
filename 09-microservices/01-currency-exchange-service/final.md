{} {}
{} {}
{} {}
{} {}
{} {}
{} {}
{} {}
{} {}
## 01-pcf-05-currency-exchange-serviceStep.md

##### /Dockerfile

```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
EXPOSE 8000
ADD target/*.jar app.jar
ENV JAVA_OPTS=""
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar" ]
```
---

##### /pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.in28minutes.microservices</groupId>
	<artifactId>05-currency-exchange-service</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
	<name>pcf-currency-exchange-service</name>

	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.7.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
		<spring-cloud.version>Greenwich.RELEASE</spring-cloud.version>
		<spring-cloud-services.version>2.1.2.RELEASE</spring-cloud-services.version>

	</properties>

	<dependencies>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-sleuth</artifactId>
		</dependency>

<!-- 		<dependency>
			<groupId>io.pivotal.spring.cloud</groupId>
			<artifactId>spring-cloud-services-starter-service-registry</artifactId>
		</dependency>
 -->

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
		</dependency>

		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>

		<dependency>
			<groupId>javax.xml.bind</groupId>
			<artifactId>jaxb-api</artifactId>
		</dependency>
		<dependency>
			<groupId>com.sun.xml.bind</groupId>
			<artifactId>jaxb-impl</artifactId>
			<version>2.3.1</version>
		</dependency>
		<dependency>
			<groupId>org.glassfish.jaxb</groupId>
			<artifactId>jaxb-runtime</artifactId>
		</dependency>
		<dependency>
			<groupId>javax.activation</groupId>
			<artifactId>activation</artifactId>
			<version>1.1.1</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

	</dependencies>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>io.pivotal.spring.cloud</groupId>
				<artifactId>spring-cloud-services-dependencies</artifactId>
				<version>${spring-cloud-services.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>${spring-cloud.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<build>
		<finalName>currency-exchange-service</finalName>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<!-- Docker -->
<!-- 			<plugin>
				<groupId>com.spotify</groupId>
				<artifactId>dockerfile-maven-plugin</artifactId>
				<version>1.4.10</version>
				<executions>
					<execution>
						<id>default</id>
						<goals>
							<goal>build</goal>
							<goal>push</goal>
						</goals>
					</execution>
				</executions>
				<configuration>
					<repository>in28min/${project.name}</repository>
					<tag>${project.version}</tag>
					<skipDockerInfo>true</skipDockerInfo>
				</configuration>
			</plugin>
 -->		</plugins>
	</build>

	<repositories>
		<repository>
			<id>spring-snapshots</id>
			<name>Spring Snapshots</name>
			<url>https://repo.spring.io/snapshot</url>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</repository>
		<repository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestone</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
	</repositories>

	<pluginRepositories>
		<pluginRepository>
			<id>spring-snapshots</id>
			<name>Spring Snapshots</name>
			<url>https://repo.spring.io/snapshot</url>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</pluginRepository>
		<pluginRepository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestone</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>
</project>
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationH2.java

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
//@EnableDiscoveryClient
public class CurrencyExchangeServiceApplicationH2 {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyExchangeServiceApplicationH2.class, args);
	}
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/CurrencyExchangeController.java

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyexchangeservice.util.environment.InstanceInformationService;

@RestController
public class CurrencyExchangeController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyExchangeController.class);

	@Autowired
	private ExchangeValueRepository repository;

	@Autowired
	private InstanceInformationService instanceInformationService;

	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public ExchangeValue retrieveExchangeValue(@PathVariable String from, @PathVariable String to,
			@RequestHeader Map<String, String> headers) {

		printAllHeaders(headers);

		ExchangeValue exchangeValue = repository.findByFromAndTo(from, to);

		LOGGER.info("{} {} {}", from, to, exchangeValue);

		if (exchangeValue == null) {
			throw new RuntimeException("Unable to find data to convert " + from + " to " + to);
		}

		exchangeValue.setExchangeEnvironmentInfo(instanceInformationService.retrieveInstanceInfo());

		return exchangeValue;
	}

	private void printAllHeaders(Map<String, String> headers) {
		headers.forEach((key, value) -> {
			LOGGER.info(String.format("Header '%s' = %s", key, value));
		});
	}
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/ExchangeValue.java

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.math.BigDecimal;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Transient;

@Entity
public class ExchangeValue {

	@Id
	private Long id;

	@Column(name = "currency_from")
	private String from;

	@Column(name = "currency_to")
	private String to;

	private BigDecimal conversionMultiple;

	@Transient
	private String exchangeEnvironmentInfo;

	public ExchangeValue() {

	}

	public ExchangeValue(Long id, String from, String to, BigDecimal conversionMultiple) {
		super();
		this.id = id;
		this.from = from;
		this.to = to;
		this.conversionMultiple = conversionMultiple;
	}

	public Long getId() {
		return id;
	}

	public String getFrom() {
		return from;
	}

	public String getTo() {
		return to;
	}

	public BigDecimal getConversionMultiple() {
		return conversionMultiple;
	}

	public String getExchangeEnvironmentInfo() {
		return exchangeEnvironmentInfo;
	}

	public void setExchangeEnvironmentInfo(String environmentInfo) {
		this.exchangeEnvironmentInfo = environmentInfo;
	}

}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/ExchangeValueRepository.java

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import org.springframework.data.jpa.repository.JpaRepository;

public interface ExchangeValueRepository extends JpaRepository<ExchangeValue, Long> {
	ExchangeValue findByFromAndTo(String from, String to);
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/security/SecurityConfiguration.java

```java
package com.in28minutes.microservices.currencyexchangeservice.security;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests().anyRequest().permitAll()
        .and()
        .httpBasic().disable()
        .csrf().disable();
  }
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/EnvironmentConfigurationLogger.java

```java
package com.in28minutes.microservices.currencyexchangeservice.util.environment;

import java.util.Arrays;
import java.util.stream.StreamSupport;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.event.ContextRefreshedEvent;
import org.springframework.context.event.EventListener;
import org.springframework.core.env.AbstractEnvironment;
import org.springframework.core.env.EnumerablePropertySource;
import org.springframework.core.env.Environment;
import org.springframework.core.env.MutablePropertySources;
import org.springframework.stereotype.Component;

@Component
public class EnvironmentConfigurationLogger {

	private static final Logger LOGGER = LoggerFactory.getLogger(EnvironmentConfigurationLogger.class);

	@SuppressWarnings("rawtypes")
	@EventListener
	public void handleContextRefresh(ContextRefreshedEvent event) {
		final Environment environment = event.getApplicationContext().getEnvironment();
		LOGGER.info("====== Environment and configuration ======");
		LOGGER.info("Active profiles: {}", Arrays.toString(environment.getActiveProfiles()));
		final MutablePropertySources sources = ((AbstractEnvironment) environment).getPropertySources();
		StreamSupport.stream(sources.spliterator(), false).filter(ps -> ps instanceof EnumerablePropertySource)
				.map(ps -> ((EnumerablePropertySource) ps).getPropertyNames()).flatMap(Arrays::stream).distinct()
				.forEach(prop -> {
					LOGGER.info("{}", prop);
//					Object resolved = environment.getProperty(prop, Object.class);
//					if (resolved instanceof String) {
//						LOGGER.info("{}", environment.getProperty(prop));
//					}
				});
		LOGGER.info("===========================================");
	}

}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/InstanceInformationService.java

```java
package com.in28minutes.microservices.currencyexchangeservice.util.environment;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
public class InstanceInformationService {

	private static final String ENV_IP_ADDRESS = "CF_INSTANCE_IP";

	private static final String DEFAULT_VALUE_IP_ADDRESS = "UNKNOWN";

	private static final String ENV_INSTANCE_GUID = "CF_INSTANCE_GUID";

	private static final String DEFAULT_ENV_INSTANCE_GUID = "UNKNOWN";

	// @Value(${ENVIRONMENT_VARIABLE_NAME:DEFAULT_VALUE})
	@Value("${" + ENV_IP_ADDRESS + ":" + DEFAULT_VALUE_IP_ADDRESS + "}")
	private String instanceIpAddress;

	@Value("${" + ENV_INSTANCE_GUID + ":" + DEFAULT_ENV_INSTANCE_GUID + "}")
	private String instanceGuid;

	public String retrieveInstanceInfo() {

		return instanceGuid + " : " + instanceIpAddress;
	}

}
```
---

##### /src/main/resources/application.properties

```properties
spring.application.name=currency-exchange-service
server.port=8000

spring.jpa.show-sql=true
spring.h2.console.enabled=true
spring.h2.console.settings.web-allow-others=true

management.endpoints.web.base-path=/manage
management.endpoints.web.exposure.include=*

#eureka.client.service-url.default-zone=http://localhost:8761/eureka

spring.security.user.name=in28minutes
spring.security.user.password=dummy
```
---

##### /src/main/resources/data.sql

```
insert into exchange_value(id,currency_from,currency_to,conversion_multiple)
values(10001,'USD','INR',65);
insert into exchange_value(id,currency_from,currency_to,conversion_multiple)
values(10002,'EUR','INR',75);
insert into exchange_value(id,currency_from,currency_to,conversion_multiple)
values(10003,'AUD','INR',25);
```
---

##### /src/test/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationTests.java

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class CurrencyExchangeServiceApplicationTests {

	@Test
	public void contextLoads() {
	}

}
```
---
## aws-ecs-fargate-03-currency-exchange-service-h2Step.md

### /pom.xml Modified
New Lines
```
	<artifactId>03-aws-currency-exchange-service-h2</artifactId>
	<name>aws-currency-exchange-service-h2</name>
		<version>2.1.1.RELEASE</version>
		<spring-cloud.version>Greenwich.RC2</spring-cloud.version>
			<version>2.3.0</version>
							<!-- <goal>push</goal> -->
		</plugins>
```
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationH2.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CurrencyExchangeServiceApplicationH2 {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyExchangeServiceApplicationH2.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/CurrencyExchangeController.java Modified
New Lines
```
import com.in28minutes.microservices.currencyexchangeservice.util.containerservice.ContainerMetaDataService;
	private ContainerMetaDataService containerMetaDataService;
		exchangeValue.setExchangeEnvironmentInfo(containerMetaDataService.retrieveContainerMetadataInfo());
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyexchangeservice.util.containerservice.ContainerMetaDataService;

@RestController
public class CurrencyExchangeController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyExchangeController.class);

	@Autowired
	private ExchangeValueRepository repository;

	@Autowired
	private ContainerMetaDataService containerMetaDataService;

	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public ExchangeValue retrieveExchangeValue(@PathVariable String from, @PathVariable String to,
			@RequestHeader Map<String, String> headers) {

		printAllHeaders(headers);

		ExchangeValue exchangeValue = repository.findByFromAndTo(from, to);

		LOGGER.info("{} {} {}", from, to, exchangeValue);

		if (exchangeValue == null) {
			throw new RuntimeException("Unable to find data to convert " + from + " to " + to);
		}

		exchangeValue.setExchangeEnvironmentInfo(containerMetaDataService.retrieveContainerMetadataInfo());

		return exchangeValue;
	}

	private void printAllHeaders(Map<String, String> headers) {
		headers.forEach((key, value) -> {
			LOGGER.info(String.format("Header '%s' = %s", key, value));
		});
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/security/SecurityConfiguration.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/containerservice/ContainerMetaDataService.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.util.containerservice;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class ContainerMetaDataService {

	private static final String ENVIRONMENT_VARIABLE_ECS_CONTAINER_METADATA_URI = "ECS_CONTAINER_METADATA_URI";

	private static final String DEFAULT_VALUE = "EMPTY";

	private static final Logger LOGGER = LoggerFactory.getLogger(ContainerMetaDataService.class);

	// @Value(${ENVIRONMENT_VARIABLE_NAME:DEFAULT_VALUE})
	@Value("${" + ENVIRONMENT_VARIABLE_ECS_CONTAINER_METADATA_URI + ":" + DEFAULT_VALUE + "}")
	private String containerMetadataUri;

	public String retrieveContainerMetadataInfo() {

		if (containerMetadataUri.contains(DEFAULT_VALUE)) {
			LOGGER.info("Environment Variable Not Available - " + ENVIRONMENT_VARIABLE_ECS_CONTAINER_METADATA_URI);
			return "NA";
		}

		return new RestTemplate().getForObject(containerMetadataUri, String.class);
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/EnvironmentConfigurationLogger.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/InstanceInformationService.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/logging/EnvironmentConfigurationLogger.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.util.logging;

import java.util.Arrays;
import java.util.stream.StreamSupport;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.event.ContextRefreshedEvent;
import org.springframework.context.event.EventListener;
import org.springframework.core.env.AbstractEnvironment;
import org.springframework.core.env.EnumerablePropertySource;
import org.springframework.core.env.Environment;
import org.springframework.core.env.MutablePropertySources;
import org.springframework.stereotype.Component;

@Component
public class EnvironmentConfigurationLogger {

	private static final Logger LOGGER = LoggerFactory.getLogger(EnvironmentConfigurationLogger.class);

	@SuppressWarnings("rawtypes")
	@EventListener
	public void handleContextRefresh(ContextRefreshedEvent event) {
		final Environment environment = event.getApplicationContext().getEnvironment();
		LOGGER.info("====== Environment and configuration ======");
		LOGGER.info("Active profiles: {}", Arrays.toString(environment.getActiveProfiles()));
		final MutablePropertySources sources = ((AbstractEnvironment) environment).getPropertySources();
		StreamSupport.stream(sources.spliterator(), false).filter(ps -> ps instanceof EnumerablePropertySource)
				.map(ps -> ((EnumerablePropertySource) ps).getPropertyNames()).flatMap(Arrays::stream).distinct()
				.forEach(prop -> LOGGER.info("{}", prop));// environment.getProperty(prop)
		LOGGER.info("===========================================");
	}

}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
spring.application.name=currency-exchange-microservice
server.servlet.context-path=/api/currency-exchange-microservice
```
## aws-ecs-fargate-04-currency-exchange-service-mysqlStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>04-aws-currency-exchange-service-mysql</artifactId>
	<name>aws-currency-exchange-service-mysql</name>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
```
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationH2.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationMySql.java New

```java
package com.in28minutes.microservices.currencyexchangeservice;

import java.math.BigDecimal;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.in28minutes.microservices.currencyexchangeservice.resource.ExchangeValue;
import com.in28minutes.microservices.currencyexchangeservice.resource.ExchangeValueRepository;

@SpringBootApplication
public class CurrencyExchangeServiceApplicationMySql implements CommandLineRunner {

	@Autowired
	ExchangeValueRepository repository;

	public static void main(String[] args) {
		SpringApplication.run(CurrencyExchangeServiceApplicationMySql.class, args);
	}

	@Override
	public void run(String... args) throws Exception {

		long count = repository.count();

		if (count == 0) {
			repository.save(new ExchangeValue(10001L, "USD", "INR", BigDecimal.valueOf(60)));
			repository.save(new ExchangeValue(10002L, "EUR", "INR", BigDecimal.valueOf(70)));
			repository.save(new ExchangeValue(10003L, "AUD", "INR", BigDecimal.valueOf(20)));
		}
	}

}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
#spring.h2.console.enabled=true
#spring.h2.console.settings.web-allow-others=true
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME:localhost}:${RDS_PORT:3306}/${RDS_DB_NAME:exchange-db}
spring.datasource.username=${RDS_USERNAME:exchange-db-user}
spring.datasource.password=${RDS_PASSWORD:dummyexchange}
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL57Dialect
```
### /src/main/resources/data.sql Deleted
### /src/test/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationTests.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class CurrencyExchangeServiceApplicationTests {

	@Test
	public void contextLoads() {
	}

}
```
---

### /src/test/resources/application.properties New

```properties
spring.jpa.hibernate.ddl-auto=create-drop
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1
spring.datasource.username=sa
spring.datasource.password=sa
```
---
## aws-ecs-fargate-06-currency-exchange-service-h2-xrayStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>06-aws-currency-exchange-service-h2-xray</artifactId>
	<name>aws-currency-exchange-service-h2-xray</name>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-xray-recorder-sdk-spring</artifactId>
```
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationH2.java New

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CurrencyExchangeServiceApplicationH2 {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyExchangeServiceApplicationH2.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationMySql.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/CurrencyExchangeController.java Modified
New Lines
```
import com.amazonaws.xray.spring.aop.XRayEnabled;
@XRayEnabled
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

import com.amazonaws.xray.spring.aop.XRayEnabled;
import com.in28minutes.microservices.currencyexchangeservice.util.containerservice.ContainerMetaDataService;

@RestController
@XRayEnabled
public class CurrencyExchangeController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyExchangeController.class);

	@Autowired
	private ExchangeValueRepository repository;

	@Autowired
	private ContainerMetaDataService containerMetaDataService;

	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public ExchangeValue retrieveExchangeValue(@PathVariable String from, @PathVariable String to,
			@RequestHeader Map<String, String> headers) {

		printAllHeaders(headers);

		ExchangeValue exchangeValue = repository.findByFromAndTo(from, to);

		LOGGER.info("{} {} {}", from, to, exchangeValue);

		if (exchangeValue == null) {
			throw new RuntimeException("Unable to find data to convert " + from + " to " + to);
		}

		exchangeValue.setExchangeEnvironmentInfo(containerMetaDataService.retrieveContainerMetadataInfo());

		return exchangeValue;
	}

	private void printAllHeaders(Map<String, String> headers) {
		headers.forEach((key, value) -> {
			LOGGER.info(String.format("Header '%s' = %s", key, value));
		});
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/xray/AwsXrayConfig.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.xray;

import javax.servlet.Filter;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.amazonaws.xray.javax.servlet.AWSXRayServletFilter;

@Configuration
public class AwsXrayConfig {

	@Bean
	public Filter TracingFilter() {
		return new AWSXRayServletFilter("currency-exchange-service");
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/xray/XRayInspector.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.xray;

import java.util.Map;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

import com.amazonaws.xray.entities.Subsegment;
import com.amazonaws.xray.spring.aop.AbstractXRayInterceptor;

@Aspect
@Component
public class XRayInspector extends AbstractXRayInterceptor {
	@Override
	protected Map<String, Map<String, Object>> generateMetadata(ProceedingJoinPoint proceedingJoinPoint,
			Subsegment subsegment) {
		return super.generateMetadata(proceedingJoinPoint, subsegment);
	}

	@Override
	@Pointcut("@within(com.amazonaws.xray.spring.aop.XRayEnabled) && bean(*)")
	public void xrayEnabledClasses() {
	}
}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
spring.h2.console.enabled=true
```
### /src/main/resources/data.sql New

```
insert into exchange_value(id,currency_from,currency_to,conversion_multiple)
values(10001,'USD','INR',65);
insert into exchange_value(id,currency_from,currency_to,conversion_multiple)
values(10002,'EUR','INR',75);
insert into exchange_value(id,currency_from,currency_to,conversion_multiple)
values(10003,'AUD','INR',25);
```
---

### /src/test/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationTests.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class CurrencyExchangeServiceApplicationTests {

	@Test
	public void contextLoads() {
	}

}
```
---
### /src/test/resources/application.properties Deleted
## docker-currency-exchange-serviceStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>05-currency-exchange-service</artifactId>
	<name>currency-exchange-service</name>
		<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
			<artifactId>spring-boot-starter-security</artifactId>
<!-- 
 		<dependency>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
 -->
<!--  
			<artifactId>spring-cloud-starter-zipkin</artifactId>
			<groupId>org.springframework.amqp</groupId>
			<artifactId>spring-rabbit</artifactId>
-->
			<version>2.3.1</version>
		<finalName>currency-exchange-service</finalName>
```
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationH2.java Modified
New Lines
```
//@EnableDiscoveryClient
```

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
//@EnableDiscoveryClient
public class CurrencyExchangeServiceApplicationH2 {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyExchangeServiceApplicationH2.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/CurrencyExchangeController.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class CurrencyExchangeController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyExchangeController.class);

	@Autowired
	private ExchangeValueRepository repository;


	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public ExchangeValue retrieveExchangeValue(@PathVariable String from, @PathVariable String to,
			@RequestHeader Map<String, String> headers) {

		printAllHeaders(headers);

		ExchangeValue exchangeValue = repository.findByFromAndTo(from, to);

		LOGGER.info("{} {} {}", from, to, exchangeValue);

		if (exchangeValue == null) {
			throw new RuntimeException("Unable to find data to convert " + from + " to " + to);
		}

		return exchangeValue;
	}

	private void printAllHeaders(Map<String, String> headers) {
		headers.forEach((key, value) -> {
			LOGGER.info(String.format("Header '%s' = %s", key, value));
		});
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/ExchangeValue.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.math.BigDecimal;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class ExchangeValue {

	@Id
	private Long id;

	@Column(name = "currency_from")
	private String from;

	@Column(name = "currency_to")
	private String to;

	private BigDecimal conversionMultiple;

	public ExchangeValue() {

	}

	public ExchangeValue(Long id, String from, String to, BigDecimal conversionMultiple) {
		super();
		this.id = id;
		this.from = from;
		this.to = to;
		this.conversionMultiple = conversionMultiple;
	}

	public Long getId() {
		return id;
	}

	public String getFrom() {
		return from;
	}

	public String getTo() {
		return to;
	}

	public BigDecimal getConversionMultiple() {
		return conversionMultiple;
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/security/SecurityConfiguration.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.security;

import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {
    http
        .authorizeRequests().anyRequest().permitAll()
        .and()
        .httpBasic().disable()
        .csrf().disable();
  }
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/containerservice/ContainerMetaDataService.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/EnvironmentConfigurationLogger.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.util.environment;

import java.util.Arrays;
import java.util.stream.StreamSupport;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.event.ContextRefreshedEvent;
import org.springframework.context.event.EventListener;
import org.springframework.core.env.AbstractEnvironment;
import org.springframework.core.env.EnumerablePropertySource;
import org.springframework.core.env.Environment;
import org.springframework.core.env.MutablePropertySources;
import org.springframework.stereotype.Component;

@Component
public class EnvironmentConfigurationLogger {

	private static final Logger LOGGER = LoggerFactory.getLogger(EnvironmentConfigurationLogger.class);

	@SuppressWarnings("rawtypes")
	@EventListener
	public void handleContextRefresh(ContextRefreshedEvent event) {
		final Environment environment = event.getApplicationContext().getEnvironment();
		LOGGER.debug("====== Environment and configuration ======");
		LOGGER.debug("Active profiles: {}", Arrays.toString(environment.getActiveProfiles()));
		final MutablePropertySources sources = ((AbstractEnvironment) environment).getPropertySources();
		StreamSupport.stream(sources.spliterator(), false).filter(ps -> ps instanceof EnumerablePropertySource)
				.map(ps -> ((EnumerablePropertySource) ps).getPropertyNames()).flatMap(Arrays::stream).distinct()
				.forEach(prop -> {
					LOGGER.debug("{}", prop);
//					Object resolved = environment.getProperty(prop, Object.class);
//					if (resolved instanceof String) {
//						LOGGER.info("{}", environment.getProperty(prop));
//					}
				});
		LOGGER.debug("===========================================");
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/logging/EnvironmentConfigurationLogger.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/xray/AwsXrayConfig.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/xray/XRayInspector.java Deleted
### /src/main/resources/application.properties Modified
New Lines
```
spring.application.name=currency-exchange-service
spring.h2.console.settings.web-allow-others=true
#logging.level.org.springframework=debug
management.endpoints.web.exposure.include=*
spring.security.user.name=in28minutes
spring.security.user.password=dummy
# Eureka
#eureka.client.service-url.default-zone=http://localhost:8761/eureka
eureka.client.service-url.defaultZone=http://naming-server:8761/eureka/
spring.sleuth.sampler.probability=1.0
#Feign and Ribbon Timeouts
feign.client.config.default.connectTimeout=50000
feign.client.config.default.readTimeout=50000
ribbon.ConnectTimeout= 60000
ribbon.ReadTimeout= 60000
# RabbitMQ
spring.rabbitmq.host=rabbitmq
```
## kubernetes-04-currency-exchange-microservice-basicStep.md

### /00-configmap-currency-conversion.yaml New

```
apiVersion: v1
data:
  YOUR_PROPERTY: value
  YOUR_PROPERTY_2: value2
kind: ConfigMap
metadata:
  name: currency-conversion
  namespace: default
```
---

### /01-hpa.yaml New

```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: currency-exchange
  namespace: default
spec:
  maxReplicas: 3
  minReplicas: 1
  scaleTargetRef:
    apiVersion: extensions/v1beta1
    kind: Deployment
    name: currency-exchange
  targetCPUUtilizationPercentage: 10
```
---

### /deployment.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: currency-exchange
  name: currency-exchange
  namespace: default
spec:
  replicas: 1 #CHANGE
  minReadySeconds: 45
  selector:
    matchLabels:
      app: currency-exchange
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: currency-exchange
    spec:
      containers:
      - image: in28min/currency-exchange:0.0.1-RELEASE #CHANGE
        imagePullPolicy: IfNotPresent
        name: currency-exchange
        ports:
        - name: liveness-port
          containerPort: 8000
        resources: #CHANGE
          requests:
            cpu: 100m
            memory: 512Mi
          limits:
            cpu: 500m
            memory: 1024Mi #256Mi 
        readinessProbe:
          httpGet:
            path: /
            port: liveness-port
          failureThreshold: 5
          periodSeconds: 10
          initialDelaySeconds: 60
        livenessProbe:
          httpGet:
            path: /
            port: liveness-port
          failureThreshold: 5
          periodSeconds: 10
          initialDelaySeconds: 60
      restartPolicy: Always
      terminationGracePeriodSeconds: 30
---
apiVersion: v1
kind: Service
metadata:
  labels: #PODS
    app: currency-exchange
  name: currency-exchange
  namespace: default
spec:
  ports:
  - # nodePort: 30702 #CHANGE
    port: 8000 #CHANGE
    protocol: TCP
    targetPort: 8000 #CHANGE
  selector:
    app: currency-exchange
  sessionAffinity: None #CHANGE
  type: LoadBalancer
```
---

### /pom.xml Modified
New Lines
```
	<artifactId>04-currency-exchange-basic</artifactId>
	<version>0.0.1-RELEASE</version> <!-- CHANGE -->
	<name>currency-exchange</name>
		<spring-cloud.version>Greenwich.SR3</spring-cloud.version>
		<dependency> <!-- CHANGE -->
		
		<finalName>currency-exchange</finalName>
				<version>1.4.6</version>
							<goal>push</goal>
```
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/CurrencyExchangeServiceApplicationH2.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CurrencyExchangeServiceApplicationH2 {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyExchangeServiceApplicationH2.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/CurrencyExchangeController.java Modified
New Lines
```
import com.in28minutes.microservices.currencyexchangeservice.util.environment.InstanceInformationService;
	private InstanceInformationService instanceInformationService;
	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	//http://localhost:8000/currency-exchange/from/USD/to/INR
		exchangeValue.setExchangeEnvironmentInfo(instanceInformationService.retrieveInstanceInfo());
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyexchangeservice.util.environment.InstanceInformationService;

@RestController
public class CurrencyExchangeController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyExchangeController.class);

	@Autowired
	private ExchangeValueRepository repository;

	@Autowired
	private InstanceInformationService instanceInformationService;

	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	}

	//http://localhost:8000/currency-exchange/from/USD/to/INR
	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public ExchangeValue retrieveExchangeValue(@PathVariable String from, @PathVariable String to,
			@RequestHeader Map<String, String> headers) {

		printAllHeaders(headers);

		ExchangeValue exchangeValue = repository.findByFromAndTo(from, to);

		LOGGER.info("{} {} {}", from, to, exchangeValue);

		if (exchangeValue == null) {
			throw new RuntimeException("Unable to find data to convert " + from + " to " + to);
		}

		exchangeValue.setExchangeEnvironmentInfo(instanceInformationService.retrieveInstanceInfo());

		return exchangeValue;
	}

	private void printAllHeaders(Map<String, String> headers) {
		headers.forEach((key, value) -> {
			LOGGER.info(String.format("Header '%s' = %s", key, value));
		});
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/ExchangeValue.java Modified
New Lines
```
	
	private String exchangeEnvironmentInfo;
	public String getExchangeEnvironmentInfo() {
		return exchangeEnvironmentInfo;
	public void setExchangeEnvironmentInfo(String exchangeEnvironmentInfo) {
		this.exchangeEnvironmentInfo = exchangeEnvironmentInfo;
	@Override
	public String toString() {
		return "ExchangeValue [id=" + id + ", from=" + from + ", to=" + to + ", conversionMultiple="
				+ conversionMultiple + ", exchangeEnvironmentInfo=" + exchangeEnvironmentInfo + "]";
	
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.math.BigDecimal;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;

@Entity
public class ExchangeValue {

	@Id
	private Long id;

	@Column(name = "currency_from")
	private String from;

	@Column(name = "currency_to")
	private String to;

	private BigDecimal conversionMultiple;
	
	private String exchangeEnvironmentInfo;

	public ExchangeValue() {

	}

	public ExchangeValue(Long id, String from, String to, BigDecimal conversionMultiple) {
		super();
		this.id = id;
		this.from = from;
		this.to = to;
		this.conversionMultiple = conversionMultiple;
	}

	public Long getId() {
		return id;
	}

	public String getFrom() {
		return from;
	}

	public String getTo() {
		return to;
	}

	public BigDecimal getConversionMultiple() {
		return conversionMultiple;
	}

	public String getExchangeEnvironmentInfo() {
		return exchangeEnvironmentInfo;
	}

	public void setExchangeEnvironmentInfo(String exchangeEnvironmentInfo) {
		this.exchangeEnvironmentInfo = exchangeEnvironmentInfo;
	}

	@Override
	public String toString() {
		return "ExchangeValue [id=" + id + ", from=" + from + ", to=" + to + ", conversionMultiple="
				+ conversionMultiple + ", exchangeEnvironmentInfo=" + exchangeEnvironmentInfo + "]";
	}
	
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/EnvironmentConfigurationLogger.java Modified
New Lines
```
		LOGGER.info("====== Environment and configuration ======");
		LOGGER.info("Active profiles: {}", Arrays.toString(environment.getActiveProfiles()));
					Object resolved = environment.getProperty(prop, Object.class);
					if (resolved instanceof String) {
						LOGGER.info("{} - {}", prop, environment.getProperty(prop));
					} else {
						LOGGER.info("{} - {}", prop, "NON-STRING-VALUE");
					}
					
```

```java
package com.in28minutes.microservices.currencyexchangeservice.util.environment;

import java.util.Arrays;
import java.util.stream.StreamSupport;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.event.ContextRefreshedEvent;
import org.springframework.context.event.EventListener;
import org.springframework.core.env.AbstractEnvironment;
import org.springframework.core.env.EnumerablePropertySource;
import org.springframework.core.env.Environment;
import org.springframework.core.env.MutablePropertySources;
import org.springframework.stereotype.Component;

@Component
public class EnvironmentConfigurationLogger {

	private static final Logger LOGGER = LoggerFactory.getLogger(EnvironmentConfigurationLogger.class);

	@SuppressWarnings("rawtypes")
	@EventListener
	public void handleContextRefresh(ContextRefreshedEvent event) {
		final Environment environment = event.getApplicationContext().getEnvironment();
		LOGGER.info("====== Environment and configuration ======");
		LOGGER.info("Active profiles: {}", Arrays.toString(environment.getActiveProfiles()));
		final MutablePropertySources sources = ((AbstractEnvironment) environment).getPropertySources();
		StreamSupport.stream(sources.spliterator(), false).filter(ps -> ps instanceof EnumerablePropertySource)
				.map(ps -> ((EnumerablePropertySource) ps).getPropertyNames()).flatMap(Arrays::stream).distinct()
				.forEach(prop -> {
					Object resolved = environment.getProperty(prop, Object.class);
					if (resolved instanceof String) {
						LOGGER.info("{} - {}", prop, environment.getProperty(prop));
					} else {
						LOGGER.info("{} - {}", prop, "NON-STRING-VALUE");
					}
					
				});
		LOGGER.debug("===========================================");
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/util/environment/InstanceInformationService.java New

```java
package com.in28minutes.microservices.currencyexchangeservice.util.environment;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
public class InstanceInformationService {

	private static final String HOST_NAME = "HOSTNAME";

	private static final String DEFAULT_ENV_INSTANCE_GUID = "UNKNOWN";

	// @Value(${ENVIRONMENT_VARIABLE_NAME:DEFAULT_VALUE})
	@Value("${" + HOST_NAME + ":" + DEFAULT_ENV_INSTANCE_GUID + "}")
	private String hostName;

	public String retrieveInstanceInfo() {
		return hostName + " v1 " + hostName.substring(hostName.length()-5);
	}

}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
spring.application.name=currency-exchange
```
## kubernetes-07-currency-exchange-microservice-stackdriverStep.md

### /00-configmap-currency-conversion.yaml Deleted
### /01-hpa.yaml Deleted
### /Dockerfile Modified
New Lines
```
#FROM openjdk:8-jdk-alpine
FROM openjdk:8
```
### /deployment.yaml Modified
New Lines
```
      - image: in28min/currency-exchange:2.0.0-RELEASE #CHANGE
        imagePullPolicy: Always #CHANGE
        env: #CHANGE
        - name: SPRING_CLOUD_GCP_TRACE_ENABLED
          value: "true"    
```
### /pom.xml Modified
New Lines
```
	<artifactId>07-currency-exchange-stackdriver</artifactId>
	<version>2.0.0-RELEASE</version> <!-- CHANGE -->
 		<dependency> <!-- CHANGE -->
			<artifactId>spring-cloud-gcp-starter-trace</artifactId>
			<artifactId>spring-cloud-gcp-starter-logging</artifactId>
```
### /src/main/java/com/in28minutes/microservices/currencyexchangeservice/resource/CurrencyExchangeController.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyexchangeservice.resource;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyexchangeservice.util.environment.InstanceInformationService;

@RestController
public class CurrencyExchangeController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyExchangeController.class);

	@Autowired
	private ExchangeValueRepository repository;

	@Autowired
	private InstanceInformationService instanceInformationService;

	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	}

	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public ExchangeValue retrieveExchangeValue(@PathVariable String from, @PathVariable String to,
			@RequestHeader Map<String, String> headers) {

		printAllHeaders(headers);

		ExchangeValue exchangeValue = repository.findByFromAndTo(from, to);

		LOGGER.info("{} {} {}", from, to, exchangeValue);

		if (exchangeValue == null) {
			throw new RuntimeException("Unable to find data to convert " + from + " to " + to);
		}

		exchangeValue.setExchangeEnvironmentInfo(instanceInformationService.retrieveInstanceInfo());

		return exchangeValue;
	}

	private void printAllHeaders(Map<String, String> headers) {
		headers.forEach((key, value) -> {
			LOGGER.info(String.format("Header '%s' = %s", key, value));
		});
	}
}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
#CHANGE - In production reduce sampling-rate to 0.01
spring.sleuth.sampler.probability=1.0
spring.cloud.gcp.trace.enabled=false
```
## kubernetes-09-currency-exchange-microservice-istioStep.md

### /Dockerfile Modified
New Lines
```
FROM openjdk:8-jdk-alpine
```
### /deployment.yaml Modified
New Lines
```
  replicas: 1
      - image: in28min/currency-exchange:3.0.0-RELEASE #CHANGE
        imagePullPolicy: Always
        env:
        - name: JAEGER_SERVICE_NAME #CHANGE
          value: currency-exchange
        - name: JAEGER_AGENT_HOST
          value: jaeger-agent.istio-system.svc.cluster.local
        - name: JAEGER_REPORTER_LOG_SPANS
          value: "true"
        - name: JAEGER_SAMPLER_TYPE
          value: const
        - name: JAEGER_SAMPLER_PARAM
          value: "1"
        - name: JAEGER_PROPAGATION
          value: b3    
  - # nodePort: 30702
    port: 8000
    targetPort: 8000
  sessionAffinity: None
  #type: LoadBalancer
```
### /pom.xml Modified
New Lines
```
	<artifactId>09-currency-exchange-istio</artifactId>
	<version>3.0.0-RELEASE</version> <!-- CHANGE -->
 
		<dependency> <!--CHANGE-->
			<groupId>io.opentracing.contrib</groupId>
			<artifactId>opentracing-spring-cloud-starter</artifactId>
			<version>0.1.7</version>
		<dependency> <!--CHANGE-->
			<groupId>io.jaegertracing</groupId>
			<artifactId>jaeger-tracerresolver</artifactId>
			<version>0.29.0</version>
```
### /src/main/resources/application.properties Modified
New Lines
```
opentracing.jaeger.probabilistic-sampler.sampling-rate=1.0 
opentracing.jaeger.enable-b3-propagation=true
```

{} {}
{} {}
{} {}
{} {}
{} {}
{} {}
{} {}
{} {}
## 01-pcf-06-currency-conversion-serviceStep.md

##### /Dockerfile

```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
EXPOSE 8100
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
	<artifactId>06-currency-conversion-service</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>
	<name>pcf-currency-conversion-service</name>

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
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-openfeign</artifactId>
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

<!-- 	<dependency>
			<groupId>io.pivotal.spring.cloud</groupId>
			<artifactId>spring-cloud-services-starter-service-registry</artifactId>
		</dependency>

		<dependency>
			<groupId>io.pivotal.spring.cloud</groupId>
			<artifactId>spring-cloud-services-starter-circuit-breaker</artifactId>
		</dependency>

		<dependency>
			<groupId>io.pivotal.spring.cloud</groupId>
			<artifactId>spring-cloud-services-starter-config-client</artifactId>
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
		<finalName>currency-conversion-service</finalName>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<!-- Docker -->
			<!-- <plugin> <groupId>com.spotify</groupId> <artifactId>dockerfile-maven-plugin</artifactId> 
				<version>1.4.10</version> <executions> <execution> <id>default</id> <goals> 
				<goal>build</goal> <goal>push</goal> </goals> </execution> </executions> 
				<configuration> <repository>in28min/${project.name}</repository> <tag>${project.version}</tag> 
				<skipDockerInfo>true</skipDockerInfo> </configuration> </plugin> -->
		</plugins>
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

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients("com.in28minutes.microservices.currencyconversionservice.resource")
//@EnableDiscoveryClient
//@EnableCircuitBreaker
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionBean.java

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

public class CurrencyConversionBean {

	private Long id;

	private String from;

	private String to;

	private BigDecimal conversionMultiple;

	private BigDecimal quantity;

	private BigDecimal totalCalculatedAmount;

	private String exchangeEnvironmentInfo;
	
	private String conversionEnvironmentInfo;

	public CurrencyConversionBean() {

	}

	public CurrencyConversionBean(Long id, String from, String to, BigDecimal conversionMultiple, BigDecimal quantity,
			BigDecimal totalCalculatedAmount, String exchangeEnvironmentInfo, String conversionEnvironmentInfo) {
		super();
		this.id = id;
		this.from = from;
		this.to = to;
		this.conversionMultiple = conversionMultiple;
		this.quantity = quantity;
		this.totalCalculatedAmount = totalCalculatedAmount;
		this.exchangeEnvironmentInfo = exchangeEnvironmentInfo;
		this.conversionEnvironmentInfo = conversionEnvironmentInfo;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFrom() {
		return from;
	}

	public void setFrom(String from) {
		this.from = from;
	}

	public String getTo() {
		return to;
	}

	public void setTo(String to) {
		this.to = to;
	}

	public BigDecimal getConversionMultiple() {
		return conversionMultiple;
	}

	public void setConversionMultiple(BigDecimal conversionMultiple) {
		this.conversionMultiple = conversionMultiple;
	}

	public BigDecimal getQuantity() {
		return quantity;
	}

	public void setQuantity(BigDecimal quantity) {
		this.quantity = quantity;
	}

	public BigDecimal getTotalCalculatedAmount() {
		return totalCalculatedAmount;
	}

	public void setTotalCalculatedAmount(BigDecimal totalCalculatedAmount) {
		this.totalCalculatedAmount = totalCalculatedAmount;
	}

	public String getExchangeEnvironmentInfo() {
		return exchangeEnvironmentInfo;
	}

	public void setExchangeEnvironmentInfo(String environmentInfo) {
		this.exchangeEnvironmentInfo = environmentInfo;
	}

	public String getConversionEnvironmentInfo() {
		return conversionEnvironmentInfo;
	}

	public void setConversionEnvironmentInfo(String conversionEnvironmentInfo) {
		this.conversionEnvironmentInfo = conversionEnvironmentInfo;
	}

}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyconversionservice.util.environment.InstanceInformationService;

@RestController
//@RefreshScope
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private InstanceInformationService instanceInformationService;

	@Autowired
	private CurrencyExchangeServiceProxy proxy;

	@GetMapping("/currency-converter/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {}. ", quantity, from, to);

		CurrencyConversionBean response = proxy.retrieveExchangeValue(from, to);

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		String conversionEnvironmentInfo = instanceInformationService.retrieveInstanceInfo();

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
	}
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

////http://localhost:8000
@FeignClient(name = "currency-exchange-service", url="${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
public interface CurrencyExchangeServiceProxy {

	///currency-exchange/from/EUR/to/INR
	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public CurrencyConversionBean retrieveExchangeValue(@PathVariable("from") String from,
			@PathVariable("to") String to);
}
```
---

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/security/SecurityConfiguration.java

```java
package com.in28minutes.microservices.currencyconversionservice.security;

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

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/EnvironmentConfigurationLogger.java

```java
package com.in28minutes.microservices.currencyconversionservice.util.environment;

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

##### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/InstanceInformationService.java

```java
package com.in28minutes.microservices.currencyconversionservice.util.environment;

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
spring.application.name=currency-conversion-service
server.port=8100

management.endpoints.web.base-path=/manage
management.endpoints.web.exposure.include=*

#eureka.client.service-url.default-zone=http://localhost:8761/eureka

#logging.level.org.springframework=debug

spring.security.user.name=in28minutes
spring.security.user.password=dummy
```
---

##### /src/test/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplicationTests.java

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class CurrencyConversionServiceApplicationTests {

	@Test
	public void contextLoads() {
	}

}
```
---
## aws-ecs-fargate-05-currency-conversion-serviceStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>05-aws-currency-conversion-service</artifactId>
	<name>aws-currency-conversion-service</name>
		<version>2.1.1.RELEASE</version>
		<spring-cloud.version>Greenwich.RC2</spring-cloud.version>
			<version>2.3.0</version>
				<groupId>com.spotify</groupId>
				<artifactId>dockerfile-maven-plugin</artifactId>
				<version>1.4.10</version>
				<executions>
					<execution>
						<id>default</id>
						<goals>
							<goal>build</goal>
							<!-- <goal>push</goal> -->
						</goals>
					</execution>
				</executions>
				<configuration>
					<repository>in28min/${project.name}</repository>
					<tag>${project.version}</tag>
					<skipDockerInfo>true</skipDockerInfo>
				</configuration>
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;
	// Spring Cloud Sleuth uses request headers to propagate trace-id & span-id.
	// Creating a bean for RestTemplate allows Spring Cloud Sleuth to inject the
	// headers into RestTemplate.
	@Bean
	public RestTemplate getRestTemplate() {
		return new RestTemplate();
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}

	// Spring Cloud Sleuth uses request headers to propagate trace-id & span-id.
	// Creating a bean for RestTemplate allows Spring Cloud Sleuth to inject the
	// headers into RestTemplate.

	@Bean
	public RestTemplate getRestTemplate() {
		return new RestTemplate();
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java Modified
New Lines
```
import java.util.HashMap;
import java.util.Map;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.ResponseEntity;
import org.springframework.web.client.RestTemplate;
import com.in28minutes.microservices.currencyconversionservice.util.containerservice.ContainerMetaDataService;
	private ContainerMetaDataService containerMetaDataService;
	@Value("${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
	private String currencyExchangeHost;
	private RestTemplate restTemplate;
		LOGGER.info("Received Request to convert from {} {} to {} ", quantity, from, to);
		ResponseEntity<CurrencyConversionBean> responseEntity = restTemplate.getForEntity(
				currencyExchangeHost + "/api/currency-exchange-microservice/currency-exchange/from/{from}/to/{to}",
				CurrencyConversionBean.class, createUriVariables(from, to));
		CurrencyConversionBean response = responseEntity.getBody();
		String conversionEnvironmentInfo = containerMetaDataService.retrieveContainerMetadataInfo();
	private Map<String, String> createUriVariables(String from, String to) {
		Map<String, String> uriVariables = new HashMap<>();
		uriVariables.put("from", from);
		uriVariables.put("to", to);
		return uriVariables;
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;
import java.util.HashMap;
import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import com.in28minutes.microservices.currencyconversionservice.util.containerservice.ContainerMetaDataService;

@RestController
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private ContainerMetaDataService containerMetaDataService;

	@Value("${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
	private String currencyExchangeHost;

	@Autowired
	private RestTemplate restTemplate;

	@GetMapping("/currency-converter/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {} ", quantity, from, to);

		ResponseEntity<CurrencyConversionBean> responseEntity = restTemplate.getForEntity(
				currencyExchangeHost + "/api/currency-exchange-microservice/currency-exchange/from/{from}/to/{to}",
				CurrencyConversionBean.class, createUriVariables(from, to));

		CurrencyConversionBean response = responseEntity.getBody();

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		String conversionEnvironmentInfo = containerMetaDataService.retrieveContainerMetadataInfo();

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
	}

	private Map<String, String> createUriVariables(String from, String to) {
		Map<String, String> uriVariables = new HashMap<>();
		uriVariables.put("from", from);
		uriVariables.put("to", to);
		return uriVariables;
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/security/SecurityConfiguration.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/containerservice/ContainerMetaDataService.java New

```java
package com.in28minutes.microservices.currencyconversionservice.util.containerservice;

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

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/EnvironmentConfigurationLogger.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/InstanceInformationService.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/logging/EnvironmentConfigurationLogger.java New

```java
package com.in28minutes.microservices.currencyconversionservice.util.logging;

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
spring.application.name=currency-conversion-microservice
server.servlet.context-path=/api/currency-conversion-microservice
```
## aws-ecs-fargate-07-currency-conversion-service-xrayStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>07-aws-currency-conversion-service-xray</artifactId>
	<name>aws-currency-conversion-service-xray</name>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-xray-recorder-sdk-spring</artifactId>
			<groupId>com.amazonaws</groupId>
			<artifactId>aws-xray-recorder-sdk-apache-http</artifactId>
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
import org.springframework.http.client.ClientHttpRequestFactory;
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import com.amazonaws.xray.proxies.apache.http.HttpClientBuilder;
	public RestTemplate restTemplate() {
		// return new RestTemplate();
		return new RestTemplate(clientHttpRequestFactory());
	private ClientHttpRequestFactory clientHttpRequestFactory() {
		HttpComponentsClientHttpRequestFactory factory = new HttpComponentsClientHttpRequestFactory(
				HttpClientBuilder.create().useSystemProperties().build());
		factory.setReadTimeout(10000);
		factory.setConnectTimeout(2000);
		factory.setConnectionRequestTimeout(2000);
		return factory;
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.http.client.ClientHttpRequestFactory;
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

import com.amazonaws.xray.proxies.apache.http.HttpClientBuilder;

@SpringBootApplication
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}

	@Bean
	public RestTemplate restTemplate() {
		// return new RestTemplate();
		return new RestTemplate(clientHttpRequestFactory());

	}

	private ClientHttpRequestFactory clientHttpRequestFactory() {
		HttpComponentsClientHttpRequestFactory factory = new HttpComponentsClientHttpRequestFactory(
				HttpClientBuilder.create().useSystemProperties().build());
		factory.setReadTimeout(10000);
		factory.setConnectTimeout(2000);
		factory.setConnectionRequestTimeout(2000);
		return factory;
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java Modified
New Lines
```
import com.amazonaws.xray.spring.aop.XRayEnabled;
@XRayEnabled
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;
import java.util.HashMap;
import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import com.amazonaws.xray.spring.aop.XRayEnabled;
import com.in28minutes.microservices.currencyconversionservice.util.containerservice.ContainerMetaDataService;

@RestController
@XRayEnabled
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private ContainerMetaDataService containerMetaDataService;

	@Value("${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
	private String currencyExchangeHost;

	@Autowired
	private RestTemplate restTemplate;

	@GetMapping("/currency-converter/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {} ", quantity, from, to);

		ResponseEntity<CurrencyConversionBean> responseEntity = restTemplate.getForEntity(
				currencyExchangeHost + "/api/currency-exchange-microservice/currency-exchange/from/{from}/to/{to}",
				CurrencyConversionBean.class, createUriVariables(from, to));

		CurrencyConversionBean response = responseEntity.getBody();

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		String conversionEnvironmentInfo = containerMetaDataService.retrieveContainerMetadataInfo();

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
	}

	private Map<String, String> createUriVariables(String from, String to) {
		Map<String, String> uriVariables = new HashMap<>();
		uriVariables.put("from", from);
		uriVariables.put("to", to);
		return uriVariables;
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/xray/AwsXrayConfig.java New

```java
package com.in28minutes.microservices.currencyconversionservice.xray;

import javax.servlet.Filter;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.amazonaws.xray.javax.servlet.AWSXRayServletFilter;

@Configuration
public class AwsXrayConfig {

	@Bean
	public Filter TracingFilter() {
		return new AWSXRayServletFilter("currency-conversion-service");

	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/xray/XRayInspector.java New

```java
package com.in28minutes.microservices.currencyconversionservice.xray;

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

## docker-currency-conversion-serviceStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>06-currency-conversion-service</artifactId>
	<name>currency-conversion-service</name>
		<version>2.1.7.RELEASE</version>
		<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
		<spring-cloud.version>Greenwich.RELEASE</spring-cloud.version>
			<artifactId>spring-cloud-starter-openfeign</artifactId>
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
		<finalName>currency-conversion-service</finalName>
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
import org.springframework.cloud.openfeign.EnableFeignClients;
@EnableFeignClients("com.in28minutes.microservices.currencyconversionservice.resource")
//@EnableDiscoveryClient
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients("com.in28minutes.microservices.currencyconversionservice.resource")
//@EnableDiscoveryClient
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionBean.java Modified
New Lines
```
			BigDecimal totalCalculatedAmount) {
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

public class CurrencyConversionBean {

	private Long id;

	private String from;

	private String to;

	private BigDecimal conversionMultiple;

	private BigDecimal quantity;

	private BigDecimal totalCalculatedAmount;


	public CurrencyConversionBean() {

	}

	public CurrencyConversionBean(Long id, String from, String to, BigDecimal conversionMultiple, BigDecimal quantity,
			BigDecimal totalCalculatedAmount) {
		super();
		this.id = id;
		this.from = from;
		this.to = to;
		this.conversionMultiple = conversionMultiple;
		this.quantity = quantity;
		this.totalCalculatedAmount = totalCalculatedAmount;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFrom() {
		return from;
	}

	public void setFrom(String from) {
		this.from = from;
	}

	public String getTo() {
		return to;
	}

	public void setTo(String to) {
		this.to = to;
	}

	public BigDecimal getConversionMultiple() {
		return conversionMultiple;
	}

	public void setConversionMultiple(BigDecimal conversionMultiple) {
		this.conversionMultiple = conversionMultiple;
	}

	public BigDecimal getQuantity() {
		return quantity;
	}

	public void setQuantity(BigDecimal quantity) {
		this.quantity = quantity;
	}

	public BigDecimal getTotalCalculatedAmount() {
		return totalCalculatedAmount;
	}

	public void setTotalCalculatedAmount(BigDecimal totalCalculatedAmount) {
		this.totalCalculatedAmount = totalCalculatedAmount;
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java Modified
New Lines
```
	private CurrencyExchangeServiceProxy proxy;
		LOGGER.info("Received Request to convert from {} {} to {}. ", quantity, from, to);
		CurrencyConversionBean response = proxy.retrieveExchangeValue(from, to);
				convertedValue);
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private CurrencyExchangeServiceProxy proxy;

	@GetMapping("/currency-converter/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {}. ", quantity, from, to);

		CurrencyConversionBean response = proxy.retrieveExchangeValue(from, to);

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java New

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "currency-exchange-service", url = "${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
//@FeignClient(name = "currency-exchange-service")
//@FeignClient(name="netflix-zuul-api-gateway-server")
public interface CurrencyExchangeServiceProxy {

	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	// @GetMapping("/currency-exchange-service/currency-exchange/from/{from}/to/{to}")
	public CurrencyConversionBean retrieveExchangeValue(@PathVariable("from") String from,
			@PathVariable("to") String to);
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/security/SecurityConfiguration.java New

```java
package com.in28minutes.microservices.currencyconversionservice.security;

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

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/containerservice/ContainerMetaDataService.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/EnvironmentConfigurationLogger.java New

```java
package com.in28minutes.microservices.currencyconversionservice.util.environment;

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

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/logging/EnvironmentConfigurationLogger.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/xray/AwsXrayConfig.java Deleted
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/xray/XRayInspector.java Deleted
### /src/main/resources/application.properties Modified
New Lines
```
spring.application.name=currency-conversion-service
management.endpoints.web.exposure.include=*
#logging.level.org.springframework=debug
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
# Zipkin
#spring.zipkin.base-url=http://zipkin-server:9411/
# RabbitMQ
spring.rabbitmq.host=rabbitmq
```
## kubernetes-05-currency-conversion-microservice-basicStep.md

### /deployment.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: currency-conversion
  name: currency-conversion
  namespace: default
spec:
  replicas: 2 #CHANGE
  minReadySeconds: 45
  selector:
    matchLabels:
      app: currency-conversion
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: currency-conversion
    spec:
      containers:
      - image: in28min/currency-conversion:0.0.2-RELEASE #CHANGE
        imagePullPolicy: IfNotPresent
        name: currency-conversion
        env:
          - name: CURRENCY_EXCHANGE_URI
            value: http://currency-exchange
#          - name: SPRING_PROFILES_ACTIVE
#            value: kubernetes
      restartPolicy: Always
      terminationGracePeriodSeconds: 30
---
apiVersion: v1
kind: Service
metadata:
  labels: #PODS
    app: currency-conversion
  name: currency-conversion
  namespace: default
spec:
  ports:
  - # nodePort: 30701 #CHANGE
    port: 8100 #CHANGE
    protocol: TCP
    targetPort: 8100 #CHANGE
  selector:
    app: currency-conversion
  sessionAffinity: None #CHANGE
  type: LoadBalancer
```
---

### /ingress.yaml New

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: gateway-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /currency-exchange/*
        backend:
          serviceName: currency-exchange
          servicePort: 8000          
      - path: /currency-conversion/*
        backend:
          serviceName: currency-conversion
          servicePort: 8100
```
---

### /pom.xml Modified
New Lines
```
	<artifactId>05-currency-conversion-basic</artifactId>
	<version>0.0.2-RELEASE</version> <!-- CHANGE -->
	<name>currency-conversion</name>
		<spring-cloud.version>Greenwich.SR3</spring-cloud.version>
		<!-- <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-netflix-ribbon</artifactId> 
			</dependency> <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-kubernetes-all</artifactId> 
			</dependency> <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-gcp-starter-logging</artifactId> 
			</dependency> -->
		<finalName>currency-conversion</finalName>
				<version>1.4.6</version>
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
//@EnableDiscoveryClient//Change
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients("com.in28minutes.microservices.currencyconversionservice.resource")
//@EnableDiscoveryClient//Change
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionBean.java Modified
New Lines
```
	private String exchangeEnvironmentInfo;
	
	private String conversionEnvironmentInfo;
			BigDecimal totalCalculatedAmount, String exchangeEnvironmentInfo, String conversionEnvironmentInfo) {
		this.exchangeEnvironmentInfo = exchangeEnvironmentInfo;
		this.conversionEnvironmentInfo = conversionEnvironmentInfo;
	public String getExchangeEnvironmentInfo() {
		return exchangeEnvironmentInfo;
	public void setExchangeEnvironmentInfo(String environmentInfo) {
		this.exchangeEnvironmentInfo = environmentInfo;
	public String getConversionEnvironmentInfo() {
		return conversionEnvironmentInfo;
	public void setConversionEnvironmentInfo(String conversionEnvironmentInfo) {
		this.conversionEnvironmentInfo = conversionEnvironmentInfo;
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

public class CurrencyConversionBean {

	private Long id;

	private String from;

	private String to;

	private BigDecimal conversionMultiple;

	private BigDecimal quantity;

	private BigDecimal totalCalculatedAmount;

	private String exchangeEnvironmentInfo;
	
	private String conversionEnvironmentInfo;

	public CurrencyConversionBean() {

	}

	public CurrencyConversionBean(Long id, String from, String to, BigDecimal conversionMultiple, BigDecimal quantity,
			BigDecimal totalCalculatedAmount, String exchangeEnvironmentInfo, String conversionEnvironmentInfo) {
		super();
		this.id = id;
		this.from = from;
		this.to = to;
		this.conversionMultiple = conversionMultiple;
		this.quantity = quantity;
		this.totalCalculatedAmount = totalCalculatedAmount;
		this.exchangeEnvironmentInfo = exchangeEnvironmentInfo;
		this.conversionEnvironmentInfo = conversionEnvironmentInfo;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getFrom() {
		return from;
	}

	public void setFrom(String from) {
		this.from = from;
	}

	public String getTo() {
		return to;
	}

	public void setTo(String to) {
		this.to = to;
	}

	public BigDecimal getConversionMultiple() {
		return conversionMultiple;
	}

	public void setConversionMultiple(BigDecimal conversionMultiple) {
		this.conversionMultiple = conversionMultiple;
	}

	public BigDecimal getQuantity() {
		return quantity;
	}

	public void setQuantity(BigDecimal quantity) {
		this.quantity = quantity;
	}

	public BigDecimal getTotalCalculatedAmount() {
		return totalCalculatedAmount;
	}

	public void setTotalCalculatedAmount(BigDecimal totalCalculatedAmount) {
		this.totalCalculatedAmount = totalCalculatedAmount;
	}

	public String getExchangeEnvironmentInfo() {
		return exchangeEnvironmentInfo;
	}

	public void setExchangeEnvironmentInfo(String environmentInfo) {
		this.exchangeEnvironmentInfo = environmentInfo;
	}

	public String getConversionEnvironmentInfo() {
		return conversionEnvironmentInfo;
	}

	public void setConversionEnvironmentInfo(String conversionEnvironmentInfo) {
		this.conversionEnvironmentInfo = conversionEnvironmentInfo;
	}

}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java Modified
New Lines
```
import com.in28minutes.microservices.currencyconversionservice.util.environment.InstanceInformationService;
	private InstanceInformationService instanceInformationService;
	
	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	//http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10
	@GetMapping("/currency-conversion/from/{from}/to/{to}/quantity/{quantity}")
		String conversionEnvironmentInfo = instanceInformationService.retrieveInstanceInfo();
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyconversionservice.util.environment.InstanceInformationService;

@RestController
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private InstanceInformationService instanceInformationService;

	@Autowired
	private CurrencyExchangeServiceProxy proxy;
	
	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	}

	//http://localhost:8100/currency-conversion/from/USD/to/INR/quantity/10
	@GetMapping("/currency-conversion/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {}. ", quantity, from, to);

		CurrencyConversionBean response = proxy.retrieveExchangeValue(from, to);

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		String conversionEnvironmentInfo = instanceInformationService.retrieveInstanceInfo();

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java Modified
New Lines
```
@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")//
//@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_SERVICE_HOST:http://localhost}:8000")
//@FeignClient(name = "currency-exchange-service")//Kubernetes Service Name
	//http://localhost:8000/currency-exchange/from/USD/to/INR
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")//
//@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_SERVICE_HOST:http://localhost}:8000")
//@FeignClient(name = "currency-exchange-service")//Kubernetes Service Name
public interface CurrencyExchangeServiceProxy {
	//http://localhost:8000/currency-exchange/from/USD/to/INR
	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public CurrencyConversionBean retrieveExchangeValue(@PathVariable("from") String from,
			@PathVariable("to") String to);
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/EnvironmentConfigurationLogger.java Modified
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
package com.in28minutes.microservices.currencyconversionservice.util.environment;

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

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/util/environment/InstanceInformationService.java New

```java
package com.in28minutes.microservices.currencyconversionservice.util.environment;

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

### /src/main/resources/application-kubernetes.properties New

```properties
CURRENCY_EXCHANGE_URI=http://currency-exchange
```
---

### /src/main/resources/application.properties Modified
New Lines
```
spring.application.name=currency-conversion
```
## kubernetes-06-currency-conversion-microservice-cloudStep.md

### /01-ingress.yaml New

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: gateway-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /currency-exchange/*
        backend:
          serviceName: currency-exchange
          servicePort: 8000          
      - path: /currency-conversion/*
        backend:
          serviceName: currency-conversion
          servicePort: 8100
```
---

### /02-rbac.yml New

```
# NOTE: The service account `default:default` already exists in k8s cluster.
# You can create a new account following like this:
#---
#apiVersion: v1
#kind: ServiceAccount
#metadata:
#  name: <new-account-name>
#  namespace: <namespace>

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: fabric8-rbac
subjects:
  - kind: ServiceAccount
    # Reference to upper's `metadata.name`
    name: default
    # Reference to upper's `metadata.namespace`
    namespace: default
roleRef:
  kind: ClusterRole
  name: view
  apiGroup: rbac.authorization.k8s.io
```
---

### /deployment.yaml Modified
New Lines
```
  replicas: 2
      - image: in28min/currency-conversion:0.0.5-RELEASE #CHANGE
#        env: //CHANGE
#          - name: CURRENCY_EXCHANGE_URI
#            value: http://currency-exchange
  - # nodePort: 30701 
    port: 8100 
    targetPort: 8100 
  sessionAffinity: None 
  type: NodePort
```
### /ingress.yaml Deleted
### /pom.xml Modified
New Lines
```
	<artifactId>06-currency-conversion-cloud</artifactId>
	<version>0.0.5-RELEASE</version> <!-- CHANGE -->
		<dependency> <!-- CHANGE -->
			<artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
		<dependency> <!-- CHANGE -->
			<artifactId>spring-cloud-starter-kubernetes-all</artifactId>
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
@EnableDiscoveryClient//Change
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients("com.in28minutes.microservices.currencyconversionservice.resource")
@EnableDiscoveryClient//Change
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java Modified
New Lines
```
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import com.in28minutes.microservices.currencyconversionservice.util.environment.InstanceInformationService;

@RestController
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private InstanceInformationService instanceInformationService;

	@Autowired
	private CurrencyExchangeServiceProxy proxy;
	
	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	}

	@GetMapping("/currency-conversion/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {}. ", quantity, from, to);

		CurrencyConversionBean response = proxy.retrieveExchangeValue(from, to);

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		String conversionEnvironmentInfo = instanceInformationService.retrieveInstanceInfo();

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java Modified
New Lines
```
import org.springframework.cloud.netflix.ribbon.RibbonClient;
//@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")//http://currency-exchange
@FeignClient(name = "currency-exchange")//Kubernetes Service Name
@RibbonClient(name = "currency-exchange")
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import org.springframework.cloud.netflix.ribbon.RibbonClient;
import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

//@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")//http://currency-exchange
//@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_SERVICE_HOST:http://localhost}:8000")
@FeignClient(name = "currency-exchange")//Kubernetes Service Name
@RibbonClient(name = "currency-exchange")
public interface CurrencyExchangeServiceProxy {
	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public CurrencyConversionBean retrieveExchangeValue(@PathVariable("from") String from,
			@PathVariable("to") String to);
}
```
---

### /src/main/resources/application-kubernetes.properties Deleted
## kubernetes-08-currency-conversion-microservice-stackdriverStep.md

### /01-ingress.yaml Deleted
### /02-rbac.yml Deleted
### /Dockerfile Modified
New Lines
```
FROM openjdk:8
#FROM openjdk:8-jdk-alpine
```
### /deployment.yaml Modified
New Lines
```
  replicas: 1 #CHANGE
      - image: in28min/currency-conversion:2.0.0-RELEASE #CHANGE
        imagePullPolicy: Always #CHANGE
        env:   #CHANGE
          - name: CURRENCY_EXCHANGE_URI
            value: http://currency-exchange
          - name: SPRING_CLOUD_GCP_TRACE_ENABLED
            value: "true"
  - # nodePort: 30701 #CHANGE
    port: 8100 #CHANGE
    targetPort: 8100 #CHANGE
  sessionAffinity: None #CHANGE
  type: LoadBalancer
```
### /pom.xml Modified
New Lines
```
	<artifactId>08-currency-conversion-stackdriver</artifactId>
	<version>2.0.0-RELEASE</version> <!-- CHANGE -->
			<artifactId>spring-cloud-gcp-starter-trace</artifactId>
			<artifactId>spring-cloud-gcp-starter-logging</artifactId>
		<!-- <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-netflix-ribbon</artifactId> 
			</dependency> <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-starter-kubernetes-all</artifactId> 
			</dependency> <dependency> <groupId>org.springframework.cloud</groupId> <artifactId>spring-cloud-gcp-starter-logging</artifactId> 
			</dependency> -->
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
//@EnableDiscoveryClient//Change
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@SpringBootApplication
@EnableFeignClients("com.in28minutes.microservices.currencyconversionservice.resource")
//@EnableDiscoveryClient//Change
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java Modified
New Lines
```
@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")//http://currency-exchange
//@FeignClient(name = "currency-exchange-service")//Kubernetes Service Name
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_URI:http://localhost}:8000")//http://currency-exchange
//@FeignClient(name = "currency-exchange", url = "${CURRENCY_EXCHANGE_SERVICE_HOST:http://localhost}:8000")
//@FeignClient(name = "currency-exchange-service")//Kubernetes Service Name
public interface CurrencyExchangeServiceProxy {
	@GetMapping("/currency-exchange/from/{from}/to/{to}")
	public CurrencyConversionBean retrieveExchangeValue(@PathVariable("from") String from,
			@PathVariable("to") String to);
}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
#In production reduce sampling-rate to 0.01
#opentracing.jaeger.probabilistic-sampler.sampling-rate=1.0 
spring.sleuth.sampler.probability=1.0
#opentracing.jaeger.enable-b3-propagation=true
spring.cloud.gcp.trace.enabled=false
```
## kubernetes-10-currency-conversion-microservice-istioStep.md

### /00-configmap-currency-conversion.yaml New

```
```
---

### /Dockerfile Modified
New Lines
```
FROM openjdk:8-jdk-alpine
```
### /deployment.yaml Modified
New Lines
```
  replicas: 1
      - image: in28min/currency-conversion:3.0.0-RELEASE #CHANGE
        imagePullPolicy: Always
        - name: JAEGER_SERVICE_NAME
          value: currency-conversion   #CHANGE 
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
        - name: CURRENCY_EXCHANGE_URI
          value: http://currency-exchange:8000
  - # nodePort: 30701
    port: 8100
    targetPort: 8100 
  sessionAffinity: None
  #type: LoadBalancer
```
### /pom.xml Modified
New Lines
```
	<artifactId>10-currency-conversion-istio</artifactId>
	<version>3.0.0-RELEASE</version> <!-- CHANGE -->
		<version>2.1.3.RELEASE</version> <!-- CHANGE -->
		
		<dependency> <!-- CHANGE -->
			<groupId>io.opentracing.contrib</groupId>
			<artifactId>opentracing-spring-cloud-starter</artifactId>
			<version>0.1.7</version>
		
		<dependency> <!-- CHANGE -->
			<groupId>io.jaegertracing</groupId>
			<artifactId>jaeger-tracerresolver</artifactId>
			<version>0.29.0</version>
```
### /src/main/java/com/in28minutes/microservices/currencyconversionservice/CurrencyConversionServiceApplication.java Modified
New Lines
```
import org.springframework.context.annotation.Bean;
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;
	@Bean
	public RestTemplate restTemplate() {
		SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
		factory.setConnectTimeout(3000);
		factory.setReadTimeout(3000);
		return new RestTemplate(factory);
```

```java
package com.in28minutes.microservices.currencyconversionservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.http.client.SimpleClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class CurrencyConversionServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(CurrencyConversionServiceApplication.class, args);
	}

	@Bean
	public RestTemplate restTemplate() {

		SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();

		factory.setConnectTimeout(3000);
		factory.setReadTimeout(3000);

		return new RestTemplate(factory);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyConversionController.java Modified
New Lines
```
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.client.RestTemplate;
	private RestTemplate restTemplate;
	//CHANGE
	@Value("${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
	private String currencyExchangeBaseUrl;
		CurrencyConversionBean response = restTemplate.getForObject(
				currencyExchangeBaseUrl + "/currency-exchange/from/" + from + "/to/" + to, CurrencyConversionBean.class);
```

```java
package com.in28minutes.microservices.currencyconversionservice.resource;

import java.math.BigDecimal;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import com.in28minutes.microservices.currencyconversionservice.util.environment.InstanceInformationService;

@RestController
public class CurrencyConversionController {

	private static final Logger LOGGER = LoggerFactory.getLogger(CurrencyConversionController.class);

	@Autowired
	private InstanceInformationService instanceInformationService;

	@Autowired
	private RestTemplate restTemplate;

	
	//CHANGE
	@Value("${CURRENCY_EXCHANGE_URI:http://localhost:8000}")
	private String currencyExchangeBaseUrl;

	@GetMapping("/")
	public String imHealthy() {
		return "{healthy:true}";
	}

	@GetMapping("/currency-conversion/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversionBean convertCurrency(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		LOGGER.info("Received Request to convert from {} {} to {}. ", quantity, from, to);

		CurrencyConversionBean response = restTemplate.getForObject(
				currencyExchangeBaseUrl + "/currency-exchange/from/" + from + "/to/" + to, CurrencyConversionBean.class);

		BigDecimal convertedValue = quantity.multiply(response.getConversionMultiple());

		String conversionEnvironmentInfo = instanceInformationService.retrieveInstanceInfo();

		return new CurrencyConversionBean(response.getId(), from, to, response.getConversionMultiple(), quantity,
				convertedValue, response.getExchangeEnvironmentInfo(), conversionEnvironmentInfo);
	}
}
```
---

### /src/main/java/com/in28minutes/microservices/currencyconversionservice/resource/CurrencyExchangeServiceProxy.java Deleted
### /src/main/resources/application.properties Modified
New Lines
```
opentracing.jaeger.probabilistic-sampler.sampling-rate=1.0 
opentracing.jaeger.enable-b3-propagation=true
```

## aws-eb-04-spring-boot-web-application-mysqlStep.md

##### /pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.in28minutes.springboot.web</groupId>
	<artifactId>04-spring-boot-web-application-mysql</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.0.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
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
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap</artifactId>
			<version>3.3.6</version>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap-datepicker</artifactId>
			<version>1.0.1</version>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>1.9.1</version>
		</dependency>

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>

		<!-- AWS -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

	<repositories>
		<repository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestones</url>
		</repository>
	</repositories>

	<pluginRepositories>
		<pluginRepository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestones</url>
		</pluginRepository>
	</pluginRepositories>

</project>
```
---

##### /src/main/java/com/in28minutes/springboot/web/EnvironmentConfigurationLogger.java

```java
package com.in28minutes.springboot.web;

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

##### /src/main/java/com/in28minutes/springboot/web/SpringBootFirstWebApplication.java

```java
package com.in28minutes.springboot.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan("com.in28minutes.springboot.web")
public class SpringBootFirstWebApplication extends SpringBootServletInitializer { // AWS

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
		return application.sources(SpringBootFirstWebApplication.class);
	}

	public static void main(String[] args) {
		SpringApplication.run(SpringBootFirstWebApplication.class, args);
	}

}
```
---

##### /src/main/java/com/in28minutes/springboot/web/controller/ErrorController.java

```java
package com.in28minutes.springboot.web.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.servlet.ModelAndView;

@Controller("error")
public class ErrorController {
	
	@ExceptionHandler(Exception.class)
	public ModelAndView handleException
		(HttpServletRequest request, Exception ex){
		ModelAndView mv = new ModelAndView();

		mv.addObject("exception", ex.getLocalizedMessage());
		mv.addObject("url", request.getRequestURL());
		
		mv.setViewName("error");
		return mv;
	}

}
```
---

##### /src/main/java/com/in28minutes/springboot/web/controller/LogoutController.java

```java
package com.in28minutes.springboot.web.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.logout.SecurityContextLogoutHandler;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class LogoutController {

	@RequestMapping(value = "/logout", method = RequestMethod.GET)
	public String logout(HttpServletRequest request,
			HttpServletResponse response) {
		
		Authentication authentication = SecurityContextHolder.getContext()
				.getAuthentication();
		
		if (authentication != null) {
			new SecurityContextLogoutHandler().logout(request, response,
					authentication);
		}

		return "redirect:/";
	}
}
```
---

##### /src/main/java/com/in28minutes/springboot/web/controller/TodoController.java

```java
package com.in28minutes.springboot.web.controller;

import java.text.SimpleDateFormat;
import java.util.Date;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.propertyeditors.CustomDateEditor;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;

import com.in28minutes.springboot.web.model.Todo;
import com.in28minutes.springboot.web.service.TodoRepository;

@Controller
public class TodoController {
	
	@Autowired
	TodoRepository repository;

	@InitBinder
	public void initBinder(WebDataBinder binder) {
		// Date - dd/MM/yyyy
		SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
		binder.registerCustomEditor(Date.class, new CustomDateEditor(
				dateFormat, false));
	}

	@RequestMapping(value = "/list-todos", method = RequestMethod.GET)
	public String showTodos(ModelMap model) {
		String name = getLoggedInUserName(model);
		model.put("todos", repository.findByUser(name));
		//model.put("todos", service.retrieveTodos(name));
		return "list-todos";
	}

	private String getLoggedInUserName(ModelMap model) {
		Object principal = SecurityContextHolder.getContext()
				.getAuthentication().getPrincipal();
		
		if (principal instanceof UserDetails) {
			return ((UserDetails) principal).getUsername();
		}
		
		return principal.toString();
	}

	@RequestMapping(value = "/add-todo", method = RequestMethod.GET)
	public String showAddTodoPage(ModelMap model) {
		model.addAttribute("todo", new Todo(0, getLoggedInUserName(model),
				"Default Desc", new Date(), false));
		return "todo";
	}

	@RequestMapping(value = "/delete-todo", method = RequestMethod.GET)
	public String deleteTodo(@RequestParam int id) {

		//if(id==1)
			//throw new RuntimeException("Something went wrong");
		repository.deleteById(id);
		//service.deleteTodo(id);
		return "redirect:/list-todos";
	}

	@RequestMapping(value = "/update-todo", method = RequestMethod.GET)
	public String showUpdateTodoPage(@RequestParam int id, ModelMap model) {
		Todo todo = repository.findById(id).get();
		//Todo todo = service.retrieveTodo(id);
		model.put("todo", todo);
		return "todo";
	}

	@RequestMapping(value = "/update-todo", method = RequestMethod.POST)
	public String updateTodo(ModelMap model, @Valid Todo todo,
			BindingResult result) {

		if (result.hasErrors()) {
			return "todo";
		}

		todo.setUser(getLoggedInUserName(model));

		repository.save(todo);
		//service.updateTodo(todo);

		return "redirect:/list-todos";
	}

	@RequestMapping(value = "/add-todo", method = RequestMethod.POST)
	public String addTodo(ModelMap model, @Valid Todo todo, BindingResult result) {

		if (result.hasErrors()) {
			return "todo";
		}

		todo.setUser(getLoggedInUserName(model));
		repository.save(todo);
		/*service.addTodo(getLoggedInUserName(model), todo.getDesc(), todo.getTargetDate(),
				false);*/
		return "redirect:/list-todos";
	}
}
```
---

##### /src/main/java/com/in28minutes/springboot/web/controller/WelcomeController.java

```java
package com.in28minutes.springboot.web.controller;

import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class WelcomeController {

	@RequestMapping(value = "/", method = RequestMethod.GET)
	public String showWelcomePage(ModelMap model) {
		model.put("name", getLoggedinUserName());
		return "welcome";
	}

	private String getLoggedinUserName() {
		Object principal = SecurityContextHolder.getContext()
				.getAuthentication().getPrincipal();
		
		if (principal instanceof UserDetails) {
			return ((UserDetails) principal).getUsername();
		}
		
		return principal.toString();
	}

}
```
---

##### /src/main/java/com/in28minutes/springboot/web/model/Todo.java

```java
package com.in28minutes.springboot.web.model;

import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.validation.constraints.Size;

@Entity
public class Todo {
    
	@Id
	@GeneratedValue
	private int id;
    
	private String user;
    
    @Size(min=10, message="Enter at least 10 Characters...")
    @Column(name="description")
    private String desc;

    private Date targetDate;
    private boolean isDone;

    public Todo() {
    		super();
    }
    
    public Todo(int id, String user, String desc, Date targetDate,
            boolean isDone) {
        super();
        this.id = id;
        this.user = user;
        this.desc = desc;
        this.targetDate = targetDate;
        this.isDone = isDone;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public Date getTargetDate() {
        return targetDate;
    }

    public void setTargetDate(Date targetDate) {
        this.targetDate = targetDate;
    }

    public boolean isDone() {
        return isDone;
    }

    public void setDone(boolean isDone) {
        this.isDone = isDone;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + id;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        Todo other = (Todo) obj;
        if (id != other.id) {
            return false;
        }
        return true;
    }

    @Override
    public String toString() {
        return String.format(
                "Todo [id=%s, user=%s, desc=%s, targetDate=%s, isDone=%s]", id,
                user, desc, targetDate, isDone);
    }

}
```
---

##### /src/main/java/com/in28minutes/springboot/web/security/SecurityConfiguration.java

```java
package com.in28minutes.springboot.web.security;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class SecurityConfiguration extends WebSecurityConfigurerAdapter{
	//Create User - in28Minutes/dummy
	@Autowired
    public void configureGlobalSecurity(AuthenticationManagerBuilder auth)
            throws Exception {
        auth.inMemoryAuthentication().withUser("in28minutes").password("{noop}dummy")
                .roles("USER", "ADMIN");
    }
	
	@Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests().antMatchers("/login", "/h2-console/**").permitAll()
                .antMatchers("/", "/*todo*/**").access("hasRole('USER')").and()
                .formLogin();
        
        http.csrf().disable();
        http.headers().frameOptions().disable();
    }
}
```
---

##### /src/main/java/com/in28minutes/springboot/web/service/TodoRepository.java

```java
package com.in28minutes.springboot.web.service;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;

import com.in28minutes.springboot.web.model.Todo;

public interface TodoRepository extends JpaRepository<Todo, Integer>{
	List<Todo> findByUser(String user);
	
	//service.retrieveTodos(name)

	//service.deleteTodo(id);
	//service.retrieveTodo(id)
	//service.updateTodo(todo)
	//service.addTodo(getLoggedInUserName(model), todo.getDesc(), todo.getTargetDate(),false);
}
```
---

##### /src/main/java/com/in28minutes/springboot/web/service/TodoService.java

```java
package com.in28minutes.springboot.web.service;

import java.util.ArrayList;
import java.util.Date;
import java.util.Iterator;
import java.util.List;

import org.springframework.stereotype.Service;

import com.in28minutes.springboot.web.model.Todo;

@Service
public class TodoService {
    private static List<Todo> todos = new ArrayList<Todo>();
    private static int todoCount = 3;

    static {
        todos.add(new Todo(1, "in28minutes", "Learn Spring MVC", new Date(),
                false));
        todos.add(new Todo(2, "in28minutes", "Learn Struts", new Date(), false));
        todos.add(new Todo(3, "in28minutes", "Learn Hibernate", new Date(),
                false));
    }

    public List<Todo> retrieveTodos(String user) {
        List<Todo> filteredTodos = new ArrayList<Todo>();
        for (Todo todo : todos) {
            if (todo.getUser().equalsIgnoreCase(user)) {
                filteredTodos.add(todo);
            }
        }
        return filteredTodos;
    }
    
    public Todo retrieveTodo(int id) {
        for (Todo todo : todos) {
            if (todo.getId()==id) {
                return todo;
            }
        }
        return null;
    }

    public void updateTodo(Todo todo){
    		todos.remove(todo);
    		todos.add(todo);
    }

    public void addTodo(String name, String desc, Date targetDate,
            boolean isDone) {
        todos.add(new Todo(++todoCount, name, desc, targetDate, isDone));
    }

    public void deleteTodo(int id) {
        Iterator<Todo> iterator = todos.iterator();
        while (iterator.hasNext()) {
            Todo todo = iterator.next();
            if (todo.getId() == id) {
                iterator.remove();
            }
        }
    }
}
```
---

##### /src/main/resources/application.properties

```properties
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
logging.level.org.springframework.web=INFO

spring.jpa.show-sql=true
#spring.h2.console.enabled=true
#spring.h2.console.settings.web-allow-others=true

spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME:localhost}:${RDS_PORT:3306}/${RDS_DB_NAME:todos}
spring.datasource.username=${RDS_USERNAME:todos-user}
spring.datasource.password=${RDS_PASSWORD:dummytodos}
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL57Dialect

#awseb-e-fiwshfewnw-stack-AWSEBSecurityGroup-8BSA213MNW98
#Security Group: sg-0e0798c5e8e7a2643 -> Web Server Instance
#sg-0e0798c5e8e7a2643 -
#sg-0e0798c5e8e7a2643 

#user name - todostageuser
#Database NAme - todos_stage

#RDS_HOSTNAME-todostage.cxohavuo1efd.us-east-1.rds.amazonaws.com
#RDS_PORT-3306
#RDS_DB_NAME-todos_stage
#RDS_USERNAME-todostageuser
#RDS_PASSWORD

#sg-0d7b7ff6b6a7331cd
```
---

##### /src/main/webapp/WEB-INF/jsp/common/footer.jspf

```
<script src="webjars/jquery/1.9.1/jquery.min.js"></script>
<script src="webjars/bootstrap/3.3.6/js/bootstrap.min.js"></script>
<script
	src="webjars/bootstrap-datepicker/1.0.1/js/bootstrap-datepicker.js"></script>
<script>
	$('#targetDate').datepicker({
		format : 'dd/mm/yyyy'
	});
</script>

</body>
</html>
```
---

##### /src/main/webapp/WEB-INF/jsp/common/header.jspf

```
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<%@taglib uri="http://www.springframework.org/tags/form" prefix="form"%>

<html>

<head>
<title>First Web Application</title>
<link href="webjars/bootstrap/3.3.6/css/bootstrap.min.css"
	rel="stylesheet">

</head>

<body>
```
---

##### /src/main/webapp/WEB-INF/jsp/common/navigation.jspf

```

<nav role="navigation" class="navbar navbar-default">
	<div class="">
		<a href="http://www.in28minutes.com" class="navbar-brand">in28Minutes</a>
	</div>
	<div class="navbar-collapse">
		<ul class="nav navbar-nav">
			<li class="active"><a href="/">Home</a></li>
			<li><a href="/list-todos">Todos</a></li>
		</ul>
		<ul class="nav navbar-nav navbar-right">
			<li><a href="/logout">Logout</a></li>
		</ul>
	</div>
</nav>
```
---

##### /src/main/webapp/WEB-INF/jsp/error.jsp

```
<%@ include file="common/header.jspf"%>
<%@ include file="common/navigation.jspf"%>
<div class="container">
An exception occurred! Please contact Support!
</div>
<%@ include file="common/footer.jspf"%>
```
---

##### /src/main/webapp/WEB-INF/jsp/list-todos.jsp

```
<%@ include file="common/header.jspf" %>
<%@ include file="common/navigation.jspf" %>
	
	<div class="container">
		<table class="table table-striped">
			<caption>Your todos are</caption>
			<thead>
				<tr>
					<th>Description</th>
					<th>Target Date</th>
					<th>Is it Done?</th>
					<th></th>
					<th></th>
				</tr>
			</thead>
			<tbody>
				<c:forEach items="${todos}" var="todo">
					<tr>
						<td>${todo.desc}</td>
						<td><fmt:formatDate value="${todo.targetDate}" pattern="dd/MM/yyyy"/></td>
						<td>${todo.done}</td>
						<td><a type="button" class="btn btn-success"
							href="/update-todo?id=${todo.id}">Update</a></td>
						<td><a type="button" class="btn btn-warning"
							href="/delete-todo?id=${todo.id}">Delete</a></td>
					</tr>
				</c:forEach>
			</tbody>
		</table>
		<div>
			<a class="button" href="/add-todo">Add a Todo</a>
		</div>
	</div>
<%@ include file="common/footer.jspf" %>
```
---

##### /src/main/webapp/WEB-INF/jsp/todo.jsp

```
<%@ include file="common/header.jspf" %>
<%@ include file="common/navigation.jspf" %>
<div class="container">
	<form:form method="post" modelAttribute="todo">
		<form:hidden path="id" />
		<fieldset class="form-group">
			<form:label path="desc">Description</form:label>
			<form:input path="desc" type="text" class="form-control"
				required="required" />
			<form:errors path="desc" cssClass="text-warning" />
		</fieldset>

		<fieldset class="form-group">
			<form:label path="targetDate">Target Date</form:label>
			<form:input path="targetDate" type="text" class="form-control"
				required="required" />
			<form:errors path="targetDate" cssClass="text-warning" />
		</fieldset>

		<button type="submit" class="btn btn-success">Add</button>
	</form:form>
</div>
<%@ include file="common/footer.jspf" %>
```
---

##### /src/main/webapp/WEB-INF/jsp/welcome.jsp

```
<%@ include file="common/header.jspf"%>
<%@ include file="common/navigation.jspf"%>
<div class="container">
	Welcome ${name}!! <a href="/list-todos">Click here</a> to manage your
	todo's.
</div>
<%@ include file="common/footer.jspf"%>
```
---

##### /src/test/java/com/in28minutes/springboot/web/SpringBootFirstWebApplicationTests.java

```java
package com.in28minutes.springboot.web;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class SpringBootFirstWebApplicationTests {

	@Test
	public void contextLoads() {
	}

}
```
---

##### /src/test/resources/application.properties

```properties
spring.jpa.hibernate.ddl-auto=create-drop
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1
spring.datasource.username=sa
spring.datasource.password=sa
```
---
## azure-wa-03-todo-web-application-mysqlStep.md

### /pom.xml Modified
New Lines
```
	<artifactId>03-todo-web-application-mysql</artifactId>
	<name>todo-web-application-mysql</name>
		<version>2.1.7.RELEASE</version>
		<relativePath />
		<!-- lookup parent from repository -->
		<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
			<artifactId>spring-boot-starter-actuator</artifactId>
		<finalName>todo-web-application-mysql</finalName>
				<groupId>com.microsoft.azure</groupId>
				<artifactId>azure-webapp-maven-plugin</artifactId>
				<version>1.7.0</version>
```
### /src/main/java/com/in28minutes/springboot/web/EnvironmentConfigurationLogger.java Modified
New Lines
```
	@SuppressWarnings("rawtypes")
				.forEach(prop -> {
					LOGGER.info("{}", prop);
//					Object resolved = environment.getProperty(prop, Object.class);
//					if (resolved instanceof String) {
//						LOGGER.info("{}", environment.getProperty(prop));
//					}
				});
```

```java
package com.in28minutes.springboot.web;

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

### /src/main/java/com/in28minutes/springboot/web/SpringBootFirstWebApplication.java Modified
New Lines
```
public class SpringBootFirstWebApplication extends SpringBootServletInitializer {
```

```java
package com.in28minutes.springboot.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan("com.in28minutes.springboot.web")
public class SpringBootFirstWebApplication extends SpringBootServletInitializer {

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
		return application.sources(SpringBootFirstWebApplication.class);
	}

	public static void main(String[] args) {
		SpringApplication.run(SpringBootFirstWebApplication.class, args);
	}

}
```
---

### /src/main/java/com/in28minutes/springboot/web/model/Todo.java Modified
New Lines
```
```

```java
package com.in28minutes.springboot.web.model;

import java.util.Date;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.validation.constraints.Size;

@Entity
public class Todo {
    
	@Id
	@GeneratedValue
	private int id;
    
	private String user;
    
    @Size(min=10, message="Enter at least 10 Characters...")
    @Column(name="description")
    private String desc;

    private Date targetDate;

    private boolean isDone;

    public Todo() {
    		super();
    }
    
    public Todo(int id, String user, String desc, Date targetDate,
            boolean isDone) {
        super();
        this.id = id;
        this.user = user;
        this.desc = desc;
        this.targetDate = targetDate;
        this.isDone = isDone;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUser() {
        return user;
    }

    public void setUser(String user) {
        this.user = user;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public Date getTargetDate() {
        return targetDate;
    }

    public void setTargetDate(Date targetDate) {
        this.targetDate = targetDate;
    }

    public boolean isDone() {
        return isDone;
    }

    public void setDone(boolean isDone) {
        this.isDone = isDone;
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + id;
        return result;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        Todo other = (Todo) obj;
        if (id != other.id) {
            return false;
        }
        return true;
    }

    @Override
    public String toString() {
        return String.format(
                "Todo [id=%s, user=%s, desc=%s, targetDate=%s, isDone=%s]", id,
                user, desc, targetDate, isDone);
    }

}
```
---

### /src/main/resources/application.properties Modified
New Lines
```
management.endpoints.web.base-path=/manage
management.endpoints.web.exposure.include=*
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME:localhost}:${RDS_PORT:3306}/${RDS_DB_NAME:todos}?serverTimezone=UTC
#spring.datasource.url=jdbc:mysql://localhost:3306/todos
#spring.datasource.username=todos-user
#spring.datasource.password=dummytodos
```
### /src/main/webapp/WEB-INF/jsp/common/navigation.jspf Modified
New Lines
```
			<li class="active"><a href="./">Home</a></li>
			<li><a href="./list-todos">Todos</a></li>
			<li><a href="./logout">Logout</a></li>
```
### /src/main/webapp/WEB-INF/jsp/list-todos.jsp Modified
New Lines
```
							href="./update-todo?id=${todo.id}">Update</a></td>
							href="./delete-todo?id=${todo.id}">Delete</a></td>
			<a class="button" href="./add-todo">Add a Todo</a>
```
### /src/main/webapp/WEB-INF/jsp/welcome.jsp Modified
New Lines
```
	Welcome ${name}!! <a href="./list-todos">Click here</a> to manage your
```
## docker-03-todo-web-application-mysqlStep.md

### /Dockerfile New

```
From tomcat:8.0.51-jre8-alpine
RUN rm -rf /usr/local/tomcat/webapps/*
COPY ./target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["catalina.sh","run"]
```
---

### /docker-compose.yml New

```
version: '3.7'
# Removed subprocess.CalledProcessError: Command '['/usr/local/bin/docker-credential-desktop', 'get']' returned non-zero exit status 1
# I had this:
# cat ~/.docker/config.json
# {"auths":{},"credsStore":"", "credsStore":"desktop","stackOrchestrator":"swarm"}
# I updated to this:
# {"auths":{},"credsStore":"","stackOrchestrator":"swarm"}
services:
  todo-web-application:
    image: in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
    #build:
      #context: .
      #dockerfile: Dockerfile
    ports:
      - "8080:8080"
    restart: always
    depends_on: # Start the depends_on first
      - mysql 
    environment:
      RDS_HOSTNAME: mysql
      RDS_PORT: 3306
      RDS_DB_NAME: todos
      RDS_USERNAME: todos-user
      RDS_PASSWORD: dummytodos
    networks:
      - todo-web-application-network

  mysql:
    image: mysql:5.7
    ports:
      - "3306:3306"
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_ROOT_PASSWORD: dummypassword 
      MYSQL_USER: todos-user
      MYSQL_PASSWORD: dummytodos
      MYSQL_DATABASE: todos
    volumes:
      - mysql-database-data-volume:/var/lib/mysql
    networks:
      - todo-web-application-network  
  
# Volumes
volumes:
  mysql-database-data-volume:

networks:
  todo-web-application-network:
```
---

### /pom.xml Modified
New Lines
```
		<relativePath /> <!-- lookup parent from repository -->
		
			<!-- Dockerfile Maven -->
			<!-- https://github.com/spotify/dockerfile-maven -->
 			<plugin>
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
### /src/main/java/com/in28minutes/springboot/web/EnvironmentConfigurationLogger.java Modified
New Lines
```
		LOGGER.debug("====== Environment and configuration ======");
		LOGGER.debug("Active profiles: {}", Arrays.toString(environment.getActiveProfiles()));
					LOGGER.debug("{}", prop);
		LOGGER.debug("===========================================");
```

```java
package com.in28minutes.springboot.web;

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

### /src/main/resources/application.properties Modified
New Lines
```
spring.datasource.url=jdbc:mysql://${RDS_HOSTNAME:localhost}:${RDS_PORT:3306}/${RDS_DB_NAME:todos}
```
## kubernetes-03-todo-web-application-mysqlStep.md

### /backup/00-generated-by-kompose/mysql-database-data-volume-persistentvolumeclaim-original-generated-by-kompose.yaml New

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  creationTimestamp: null
  labels:
    io.kompose.service: mysql-database-data-volume
  name: mysql-database-data-volume
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
status: {}
```
---

### /backup/00-generated-by-kompose/mysql-deployment-original-generated-by-kompose.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.19.0 ()
  creationTimestamp: null
  labels:
    io.kompose.service: mysql
  name: mysql
spec:
  replicas: 1
  strategy:
    type: Recreate
  template:
    metadata:
      annotations:
        kompose.cmd: kompose convert
        kompose.version: 1.19.0 ()
      creationTimestamp: null
      labels:
        io.kompose.service: mysql
    spec:
      containers:
      - env:
        - name: MYSQL_DATABASE
          value: todos
        - name: MYSQL_PASSWORD
          value: dummytodos
        - name: MYSQL_ROOT_PASSWORD
          value: dummypassword
        - name: MYSQL_USER
          value: todos-user
        image: mysql:5.7
        name: mysql
        ports:
        - containerPort: 3306
        resources: {}
        volumeMounts:
        - mountPath: /var/lib/mysql
          name: mysql-database-data-volume
      restartPolicy: Always
      volumes:
      - name: mysql-database-data-volume
        persistentVolumeClaim:
          claimName: mysql-database-data-volume
status: {}
```
---

### /backup/00-generated-by-kompose/mysql-service-original-generated-by-kompose.yaml New

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.19.0 ()
  creationTimestamp: null
  labels:
    io.kompose.service: mysql
  name: mysql
spec:
  ports:
  - name: "3306"
    port: 3306
    targetPort: 3306
  selector:
    io.kompose.service: mysql
status:
  loadBalancer: {}
```
---

### /backup/00-generated-by-kompose/todo-web-application-deployment-original-generated-by-kompose.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.19.0 ()
  creationTimestamp: null
  labels:
    io.kompose.service: todo-web-application
  name: todo-web-application
spec:
  replicas: 1
  strategy: {}
  template:
    metadata:
      annotations:
        kompose.cmd: kompose convert
        kompose.version: 1.19.0 ()
      creationTimestamp: null
      labels:
        io.kompose.service: todo-web-application
    spec:
      containers:
      - env:
        - name: RDS_DB_NAME
          value: todos
        - name: RDS_HOSTNAME
          value: mysql
        - name: RDS_PASSWORD
          value: dummytodos
        - name: RDS_PORT
          value: "3306"
        - name: RDS_USERNAME
          value: todos-user
        image: in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
        name: todo-web-application
        ports:
        - containerPort: 8080
        resources: {}
      restartPolicy: Always
status: {}
```
---

### /backup/00-generated-by-kompose/todo-web-application-service-original-generated-by-kompose.yaml New

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.19.0 ()
  creationTimestamp: null
  labels:
    io.kompose.service: todo-web-application
  name: todo-web-application
spec:
  ports:
  - name: "8080"
    port: 8080
    targetPort: 8080
  selector:
    io.kompose.service: todo-web-application
status:
  loadBalancer: {}
```
---

### /backup/01-updated-manually/mysql-database-data-volume-persistentvolumeclaim.yaml New

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  creationTimestamp: null
  labels:
    io.kompose.service: mysql-database-data-volume
  name: mysql-database-data-volume
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
status: {}
```
---

### /backup/01-updated-manually/mysql-deployment.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    io.kompose.service: mysql
  name: mysql
spec:
  replicas: 1
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        io.kompose.service: mysql
    spec:
      containers:
      - env:
        - name: MYSQL_DATABASE
          value: todos
        - name: MYSQL_PASSWORD
          value: dummytodos
        - name: MYSQL_ROOT_PASSWORD
          value: dummypassword
        - name: MYSQL_USER
          value: todos-user
        image: mysql:5.7
        args:
          - "--ignore-db-dir=lost+found" #CHANGE
        name: mysql
        ports:
        - containerPort: 3306
        volumeMounts:
        - mountPath: /var/lib/mysql
          name: mysql-database-data-volume
      restartPolicy: Always
      volumes:
      - name: mysql-database-data-volume
        persistentVolumeClaim:
          claimName: mysql-database-data-volume
```
---

### /backup/01-updated-manually/mysql-service.yaml New

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.19.0 ()
  creationTimestamp: null
  labels:
    io.kompose.service: mysql
  name: mysql
spec:
  type: LoadBalancer
  ports:
  - name: "3306"
    port: 3306
    targetPort: 3306
  selector:
    io.kompose.service: mysql
```
---

### /backup/01-updated-manually/todo-web-application-deployment.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    io.kompose.service: todo-web-application
  name: todo-web-application
spec:
  replicas: 1
  template:
    metadata:
      labels:
        io.kompose.service: todo-web-application
    spec:
      containers:
      - env:
        - name: RDS_DB_NAME
          value: todos
        - name: RDS_HOSTNAME
          value: mysql
        - name: RDS_PASSWORD
          value: dummytodos
        - name: RDS_PORT
          value: "3306"
        - name: RDS_USERNAME
          value: todos-user
        image: in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
        name: todo-web-application
        ports:
        - containerPort: 8080
      restartPolicy: Always
```
---

### /backup/01-updated-manually/todo-web-application-service.yaml New

```
apiVersion: v1
kind: Service
metadata:
  labels:
    io.kompose.service: todo-web-application
  name: todo-web-application
spec:
  type: LoadBalancer
  ports:
  - name: "8080"
    port: 8080
    targetPort: 8080
  selector:
    io.kompose.service: todo-web-application
```
---

### /backup/02-final-backup-at-end-of-course/mysql-database-data-volume-persistentvolumeclaim.yaml New

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  creationTimestamp: null
  labels:
    io.kompose.service: mysql-database-data-volume
  name: mysql-database-data-volume
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 100Mi
status: {}
```
---

### /backup/02-final-backup-at-end-of-course/mysql-deployment.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    io.kompose.service: mysql
  name: mysql
spec:
  replicas: 1
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        io.kompose.service: mysql
    spec:
      containers:
      - env:
        - name: MYSQL_DATABASE
          value: todos
        - name: MYSQL_PASSWORD
          value: dummytodos
        - name: MYSQL_ROOT_PASSWORD
          value: dummypassword
        - name: MYSQL_USER
          value: todos-user
        image: mysql:5.7
        args:
          - "--ignore-db-dir=lost+found" #CHANGE
        name: mysql
        ports:
        - containerPort: 3306
        volumeMounts:
        - mountPath: /var/lib/mysql
          name: mysql-database-data-volume
      restartPolicy: Always
      volumes:
      - name: mysql-database-data-volume
        persistentVolumeClaim:
          claimName: mysql-database-data-volume
```
---

### /backup/02-final-backup-at-end-of-course/mysql-service.yaml New

```
apiVersion: v1
kind: Service
metadata:
  annotations:
    kompose.cmd: kompose convert
    kompose.version: 1.19.0 ()
  creationTimestamp: null
  labels:
    io.kompose.service: mysql
  name: mysql
spec:
  type: ClusterIP
  ports:
  - name: "3306"
    port: 3306
    targetPort: 3306
  selector:
    io.kompose.service: mysql
```
---

### /backup/02-final-backup-at-end-of-course/todo-web-application-deployment.yaml New

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    io.kompose.service: todo-web-application
  name: todo-web-application
spec:
  replicas: 1
  template:
    metadata:
      labels:
        io.kompose.service: todo-web-application
    spec:
      containers:
      - env:
        - name: RDS_DB_NAME
#          value: todos
          valueFrom:
            configMapKeyRef:
              key: RDS_DB_NAME
              name: todo-web-application-config
        - name: RDS_HOSTNAME
          valueFrom:
            configMapKeyRef:
              key: RDS_HOSTNAME
              name: todo-web-application-config
        - name: RDS_PASSWORD
          valueFrom:
            secretKeyRef:
              key: RDS_PASSWORD
              name: todo-web-application-secrets
        - name: RDS_PORT
          valueFrom:
            configMapKeyRef:
              key: RDS_PORT
              name: todo-web-application-config
        - name: RDS_USERNAME
          valueFrom:
            configMapKeyRef:
              key: RDS_USERNAME
              name: todo-web-application-config
        image: in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
        name: todo-web-application
        ports:
        - containerPort: 8080
      restartPolicy: Always
```
---

### /backup/02-final-backup-at-end-of-course/todo-web-application-service.yaml New

```
apiVersion: v1
kind: Service
metadata:
  labels:
    io.kompose.service: todo-web-application
  name: todo-web-application
spec:
  type: LoadBalancer
  ports:
  - name: "8080"
    port: 8080
    targetPort: 8080
  selector:
    io.kompose.service: todo-web-application
```
---

### /docker-compose.yml Modified
New Lines
```
```
### /pom.xml Modified
New Lines
```
				<version>1.4.6</version>
					<!-- todo -->
```
### /src/main/java/com/in28minutes/springboot/web/EnvironmentConfigurationLogger.java Modified
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
package com.in28minutes.springboot.web;

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

## pcf-03-todo-web-application-mysqlStep.md

### /Dockerfile Deleted
### /backup/00-generated-by-kompose/mysql-database-data-volume-persistentvolumeclaim-original-generated-by-kompose.yaml Deleted
### /backup/00-generated-by-kompose/mysql-deployment-original-generated-by-kompose.yaml Deleted
### /backup/00-generated-by-kompose/mysql-service-original-generated-by-kompose.yaml Deleted
### /backup/00-generated-by-kompose/todo-web-application-deployment-original-generated-by-kompose.yaml Deleted
### /backup/00-generated-by-kompose/todo-web-application-service-original-generated-by-kompose.yaml Deleted
### /backup/01-updated-manually/mysql-database-data-volume-persistentvolumeclaim.yaml Deleted
### /backup/01-updated-manually/mysql-deployment.yaml Deleted
### /backup/01-updated-manually/mysql-service.yaml Deleted
### /backup/01-updated-manually/todo-web-application-deployment.yaml Deleted
### /backup/01-updated-manually/todo-web-application-service.yaml Deleted
### /backup/02-final-backup-at-end-of-course/mysql-database-data-volume-persistentvolumeclaim.yaml Deleted
### /backup/02-final-backup-at-end-of-course/mysql-deployment.yaml Deleted
### /backup/02-final-backup-at-end-of-course/mysql-service.yaml Deleted
### /backup/02-final-backup-at-end-of-course/todo-web-application-deployment.yaml Deleted
### /backup/02-final-backup-at-end-of-course/todo-web-application-service.yaml Deleted
### /docker-compose.yml Deleted
### /pom.xml Modified
New Lines
```
<!--  		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-cloudfoundry-connector</artifactId>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-spring-service-connector</artifactId>
 -->
```
### /src/main/java/com/in28minutes/springboot/web/CloudFoundryDatabaseConfig.java New

```java
package com.in28minutes.springboot.web;

//import javax.sql.DataSource;
//
//import org.springframework.cloud.Cloud;
//import org.springframework.cloud.CloudFactory;
//import org.springframework.context.annotation.Bean;
//import org.springframework.context.annotation.Configuration;
//import org.springframework.context.annotation.Profile;

//@Configuration
//@Profile("cloud")
//public  class CloudFoundryDatabaseConfig {
// 
//    @Bean
//    public Cloud cloud() {
//        return new CloudFactory().getCloud();
//    }
// 
//    @Bean
//    public DataSource dataSource() {
//        DataSource dataSource = 
//        		cloud().getServiceConnector("todo-database", DataSource.class, null);
//        //Customized
//        return dataSource;
//    }
//}
```
---

### /src/main/java/com/in28minutes/springboot/web/EnvironmentConfigurationLogger.java Modified
New Lines
```
					LOGGER.info("{}", prop);
//					Object resolved = environment.getProperty(prop, Object.class);
//					if (resolved instanceof String) {
//						LOGGER.info("{}", environment.getProperty(prop));
//					}
		LOGGER.info("===========================================");
```

```java
package com.in28minutes.springboot.web;

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

### /src/main/resources/application.properties Modified
New Lines
```
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL55Dialect
spring.datasource.url=jdbc:mysql://localhost:3306/todos
spring.datasource.username=todos-user
spring.datasource.password=dummytodos
#spring.datasource.url=${vcap.services.todo-database.credentials.jdbcUrl}
#spring.datasource.username=${vcap.services.todo-database.credentials.username:todos-user}
#spring.datasource.password=${vcap.services.todo-database.credentials.password:dummytodos}
#spring.datasource.url=jdbc:mysql://${cloud.services.mysql.connection.host:localhost}:${cloud.services.mysql.connection.port:3306}/${cloud.services.mysql.connection.path:todos}
#spring.datasource.url=${cloud.services.mysql.connection.jdbcurl}
```
### /src/main/webapp/WEB-INF/jsp/common/navigation.jspf Modified
New Lines
```
			<li class="active"><a href="/">Home</a></li>
			<li><a href="/list-todos">Todos</a></li>
			<li><a href="/logout">Logout</a></li>
```
### /src/main/webapp/WEB-INF/jsp/list-todos.jsp Modified
New Lines
```
							href="/update-todo?id=${todo.id}">Update</a></td>
							href="/delete-todo?id=${todo.id}">Delete</a></td>
			<a class="button" href="/add-todo">Add a Todo</a>
```
### /src/main/webapp/WEB-INF/jsp/welcome.jsp Modified
New Lines
```
	Welcome ${name}!! <a href="/list-todos">Click here</a> to manage your
```

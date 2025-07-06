# SpringBoot
e##############################
What is Spring boot
################################

Spring Boot is an open-source Java-based framework used to create stand-alone applications quickly and easily with minimal configuration.


Spring Boot Stand-Alone Application:
-------------------------------------------
-> You just run a main() method like any regular Java program.
-> It includes an embedded server (e.g., Tomcat) in the application itself.
-> No need to install or configure any external server separately.



Example:
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);  // Starts embedded server
    }
}

When you run above program
	a. Starts up the embedded server.
	b. Deploys your app on it.
	c. Listens to HTTP requests (like a web server).

Note: Since Spring boot is stand alone application, we package the application as jar file


##########################
What is Spring MVC?
##########################
-> Spring MVC stands for Spring Model-View-Controller.
—> It's a web framework in the Spring used to build web applications using the MVC design pattern.

How Spring MVC Works:
--------------------------
a. User sends a request (/home) or from html form.
b. DispatcherServlet (front controller) catches the request.
c. Finds the matching controller method using annotations like @GetMapping.
d. Executes logic and prepares data (Model).
c. Returns a view.


Spring Boot Annotations
---------------------------
1. What is @RequestMapping?

a. It’s a Spring MVC annotation used to map HTTP requests to handler methods or classes in your controller.
b. It tells Spring which URL (path) a particular method or class should handle.


2. What is @RequestParam?

a. It’s an annotation used in controller method parameters.
b. It binds HTTP request parameters (query parameters or form data) to method arguments.
c. It extracts values from the URL or form and passes them into your controller method.

3. What is @Service?

a. @Service is a Spring stereotype annotation.
b. It marks a class as a Service component in the Service Layer of your application.
c. Spring will detect it during component scanning and create an instance (bean) of that class in the Spring IOC container
d. It makes your class eligible for dependency injection.


4. What is @Component?

a. @Component is a generic Spring annotation to mark a class as a Spring-managed bean.
b. When you annotate a class with @Component, Spring will detect it during component scanning and create an instance (bean) of that class in the Spring IOC container.
c. It makes your class eligible for dependency injection.

5. What is @Controller?

a. @Controller is a Spring MVC stereotype annotation used to mark a class as a web controller.
b. Acts as mediator layer in MVC Architecture 

6. What is Model?

a. It's part of the MVC (Model-View-Controller) pattern.
b. It allows the controller to send data (attributes) to the view.
c. The view can then access and display that data.
d. Model in Spring is an interface, not a class

7. What is ModelMap in Spring MVC?

a. ModelMap is a special object provided by Spring MVC to pass data from your controller to the view layer (like JSP, Thymeleaf, etc.).
b. It behaves like a Map (key-value pairs)


######################################
Example - Create JSP page inside /src/webapp/WEB-INF/views/ and call that from controller layer 
###################################
Step 1:
----------
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Register</title>
</head>
<body>
	<h2>Register here......</h2>
</body>
</html>
registration.jsp
--------------------------------------------------------------------------------
Step 2:
-------
spring.application.name=app

spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
application.properties
-----------------------------------------------------------------------------------
Step 3: Add dependency
-----------
<dependency>
		    <groupId>org.apache.tomcat.embed</groupId>
		    <artifactId>tomcat-embed-jasper</artifactId>
</dependency>
------------------------------------------------------------------------------------
Step 4: Developer Controller Layer

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class EmployeeController {
	
	@RequestMapping("/view")
	public String viewRegisterEmp() {
		return "register";
	}

}
Note
-------------
Why do we use a Dialect in Spring Boot (Hibernate)?
The dialect tells Hibernate how to generate SQL optimized for a specific database (like MySQL, PostgreSQL, Oracle, etc.).
------------------------
######################################
Example - Submit data from .jsp file and read that in Controller layer
###################################

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Register</title>
</head>
<body>
	<h2>Register here..</h2>
	<form action="saveReg" method="post">
		<pre>
			First Name <input type="text" name="firstName"/>
			Last Name <input type="text" name="lastName"/>
			Email Id <input type="text" name="email"/>
			Mobile <input type="text" name="mobile"/>
			<input type="submit" value="save"/>
		</pre>
	</form>
${msg}
</body>
</html>
registration.jsp
-----------------------------------------------------------------------------

package com.app.entity;
import jakarta.persistence.*;

@Entity
public class Employee {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;
	
	@Column(name = "first_name", nullable = false)
	private String firstName;
	
	@Column(name = "last_name", nullable = false)
	private String lastName;
	
	@Column(name = "email", nullable = false,unique = true)
	private String email;
	
	@Column(name = "mobile", nullable = false, unique = true)
	private String mobile;

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getMobile() {
		return mobile;
	}

	public void setMobile(String mobile) {
		this.mobile = mobile;
	}
	
	
}
Employee.java ---- Entity Class
------------------------------------------------------------------

spring.application.name=app

spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

spring.datasource.url=jdbc:mysql://localhost:3306/appdb
spring.datasource.username=root
spring.datasource.password=test

spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect

spring.jpa.hibernate.ddl-auto=update

spring.jpa.show-sql=true

Add application.properties files
----------------------------------------

package com.app.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import com.app.entity.Employee;

@Controller
public class EmployeeController {

	//Handler Methods - http://localhost:8080/view
	@RequestMapping("/view")
	public String viewRegisterPage() {
		return "registration";//--> RequestDispatcher
	}
	
	@RequestMapping("/saveReg")
	public String getRegistrationData(Employee employee, Model model) {
		employeeService.saveEmployeeDetails(employee);
		model.addAttribute("msg", "Record is saved");
		return "registration";
	}
}
EmployeeController.java
---------------------------------------------------------
import org.springframework.data.jpa.repository.JpaRepository;

import com.app.entity.Employee;

public interface EmployeeRepository extends JpaRepository<Employee, Long> {

}
Repository Layer
----------------------------------
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.app.entity.Employee;
import com.app.repository.EmployeeRepository;

@Service
public class EmployeeService {
	
	@Autowired
	private EmployeeRepository employeeRepository;
	
	public void saveEmployeeDetails(Employee employee) {
		employeeRepository.save(employee);
	}

}
EmployeeService.java

_________________________________________
What is @ModelAttribute in Spring MVC?
@ModelAttribute is an annotation used in Spring MVC to bind form data or request parameters to Java objects
_______________________________________

#############################
Payload / DTO implementation in Spring boot
############################
Step 1: Modify registration.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Register</title>
</head>
<body>
	<h2>Register here..</h2>
	<form action="saveReg" method="post">
		<pre>
			First Name <input type="text" name="firstName"/>
			Last Name <input type="text" name="lastName"/>
			Email Id <input type="text" name="email"/>
			Mobile <input type="text" name="mobile"/>
			City <input type="text" name="city"/>
			State <input type="text" name="state"/>
			Pincode <input type="text" name="pinCode"/>
			Address Line <input type="text" name="addressLine"/>
			<input type="submit" value="save"/>
		</pre>
	</form>
	${msg}
</body>
</html>

Step 2: Modify EmployeeController.java
@Controller
public class EmployeeController {
	
	@Autowired
	private EmployeeService employeeService;

	//Handler Methods - http://localhost:8080/view
	@RequestMapping("/view")
	public String viewRegisterPage() {
		return "registration";//--> RequestDispatcher
	}
	
//	@RequestMapping("/saveReg")
//	public String getRegistrationData(Employee employee, ModelMap model) {
//		employeeService.saveEmployeeDetails(employee);
//		model.addAttribute("msg", "Record is saved");
//		return "registration";
//	}
	
	@RequestMapping("/saveReg")
	public String getRegistrationData(@ModelAttribute EmployeeDto employeeDto, Model model) {
		employeeService.saveEmployeeDetails(employeeDto);
		model.addAttribute("msg", "Record is saved");
        return "registration";
	}
	
}
Step 3: Add Address Entity
---------------------------
package com.app.entity;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Address {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;
	private String city;
	private String state;
	private int pinCode;
	private String addressLine;
	public long getId() {
		return id;
	}
	public void setId(long id) {
		this.id = id;
	}
	public String getCity() {
		return city;
	}
	public void setCity(String city) {
		this.city = city;
	}
	public String getState() {
		return state;
	}
	public void setState(String state) {
		this.state = state;
	}
	public int getPinCode() {
		return pinCode;
	}
	public void setPinCode(int pinCode) {
		this.pinCode = pinCode;
	}
	public String getAddressLine() {
		return addressLine;
	}
	public void setAddressLine(String addressLine) {
		this.addressLine = addressLine;
	}
	
	
}

Step 4: Add Repository Layer - AddressRepository.java
-----------------------------

Step 5: Modify Service Layer - EmployeeService.java
--------------------------
@Service
public class EmployeeService {
	
	@Autowired
	private EmployeeRepository employeeRepository;
	
	@Autowired
	private AddressRepository addressRepository;
	
	public void saveEmployeeDetails(EmployeeDto employeeDto) {
		Employee emp = new Employee();
		BeanUtils.copyProperties(employeeDto,emp);//-->Copy Data From Dto to Entity
		employeeRepository.save(emp);
		
		Address address = new Address();
		BeanUtils.copyProperties(employeeDto,address);//-->Copy Data From Dto to Entity
		addressRepository.save(address);
	}

}

#############################
Read Data from form using @RequestParam Annotation
############################

Step 1: Create employee.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Employee</title>
</head>
<body>
	<h2>Employee Registration</h2>
	<form action="saveRegistration" method="post">
		<pre>
			First Name <input type="text" name="firstName"/>
			Last Name <input type="text" name="lastName"/>
			Email Id <input type="text" name="emailId"/>
			Mobile <input type="text" name="mobile"/>
			<input type="submit" value="save"/>
		</pre>
	</form>
</body>
</html>

Step 2: Modify EmployeeController.java

@RequestMapping("/saveRegistration")
	public String getDataUsingRequestParam(
			 @RequestParam String firstName,
			 @RequestParam String lastName,
			 @RequestParam("emailId") String email,
			 @RequestParam String mobile
			) {
		
		employeeService.register(firstName,lastName,email,mobile);
		return "employee";
	}
	
	//Handler Methods - http://localhost:8080/viewRegistration
		@RequestMapping("/viewRegistration")
		public String viewRegister() {
			return "employee";//--> RequestDispatcher
		}
	

Step 3: Modify EmployeeService.java

public void register(String firstName, String lastName, String email, String mobile) {
		Employee emp = new Employee();
		emp.setFirstName(firstName);
		emp.setLastName(lastName);
		emp.setEmail(email);
		emp.setMobile(mobile);
		employeeRepository.save(emp);
		
}

#######################################
what is JSTL tags?
-> Using JSTL we can write java code in the form of tags inside JSP pages
-> This makes non java developer to embed java code inside JSP

Example:
<c:out value="hello"></c:out>
#######################################

Steps to configure JSTL in spring boot version 3.x
---------------------------------------------------

1. Modify and add the below mentioned dependency

          <packaging>war</packaging>

 	   <dependency>
	        <groupId>jakarta.servlet.jsp.jstl</groupId>
	        <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
	        <version>3.0.0</version>
	    </dependency>
	
	    <dependency>
	        <groupId>org.glassfish.web</groupId>
	        <artifactId>jakarta.servlet.jsp.jstl</artifactId>
	        <version>3.0.1</version>
	    </dependency>

		 <dependency>
		        <groupId>jakarta.servlet</groupId>
		        <artifactId>jakarta.servlet-api</artifactId>
		        <scope>provided</scope>
		 </dependency>

2. Modify your main spring boot class:

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class AppApplication extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(AppApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(AppApplication.class);
    }
}

3. Add core tag lib inside .jsp page
<%@ taglib prefix="e" uri="http://java.sun.com/jsp/jstl/core" %>

4. develop employee.jsp and add the following code
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Employee</title>
</head>
<body>
	<h2>Employee Registration</h2>
	<c:out value="hello"></c:out>	
</body>
</html>
5. In your controller develop the following method
	//Handler Methods - http://localhost:8080/viewRegistration
		@RequestMapping("/viewRegistration")
		public String viewRegister() {
			return "employee";//--> RequestDispatcher
		}
###################
Adding Menu.jsp
######################

Step 1: create menu.jsp page as shown below inside /WEB-INF/views
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
	<a href="view">Register Employee</a>
	<a href="">List Employees</a>
</body>
</html>

Step 2: Include menu in registration.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ include file="menu.jsp" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Register</title>
</head>
<body>
	<h2>Register here..</h2>
	<form action="saveReg" method="post">
		<pre>
			First Name <input type="text" name="firstName"/>
			Last Name <input type="text" name="lastName"/>
			Email Id <input type="text" name="email"/>
			Mobile <input type="text" name="mobile"/>
			City <input type="text" name="city"/>
			State <input type="text" name="state"/>
			Pincode <input type="text" name="pinCode"/>
			Address Line <input type="text" name="addressLine"/>
			<input type="submit" value="save"/>
		</pre>
	</form>
	${msg}
</body>
</html>

#############################
Read data and build html table using JSTL concept
###########################

Step 1: 
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
</head>
<body>
	<a href="view">Register Employee</a>
	<a href="listRegistrations">List Employees</a>
</body>
</html>

Step 2: EmployeeController.java

package com.app.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.app.dto.EmployeeDto;
import com.app.entity.Employee;
import com.app.repository.AddressRepository;
import com.app.service.EmployeeService;


@Controller
public class EmployeeController {

    private final AddressRepository addressRepository;
	
	@Autowired
	private EmployeeService employeeService;

    EmployeeController(AddressRepository addressRepository) {
        this.addressRepository = addressRepository;
    }

	//Handler Methods - http://localhost:8080/view
	@RequestMapping("/view")
	public String viewRegisterPage() {
		return "registration";//--> RequestDispatcher
	}
	
//	@RequestMapping("/saveReg")
//	public String getRegistrationData(Employee employee, ModelMap model) {
//		employeeService.saveEmployeeDetails(employee);
//		model.addAttribute("msg", "Record is saved");
//		return "registration";
//	}
	
	@RequestMapping("/saveReg")
	public String getRegistrationData(@ModelAttribute EmployeeDto employeeDto, Model model) {
		try {
			employeeService.saveEmployeeDetails(employeeDto);
			model.addAttribute("msg", "Record is saved");
	        return "registration";
		} catch (Exception e) {
			model.addAttribute("msg", "Duplicate entry");
	        return "registration";
		}
	}
	
	@RequestMapping("/saveRegistration")
	public String getDataUsingRequestParam(
			 @RequestParam String firstName,
			 @RequestParam String lastName,
			 @RequestParam("emailId") String email,
			 @RequestParam String mobile
			) {
		
		employeeService.register(firstName,lastName,email,mobile);
		return "employee";
	}
	
	//Handler Methods - http://localhost:8080/viewRegistration
		@RequestMapping("/viewRegistration")
		public String viewRegister() {
			return "employee";//--> RequestDispatcher
		}
		

		//Handler Methods - http://localhost:8080/listRegistrations
			@RequestMapping("/listRegistrations")
			public String viewRegistrations(ModelMap model) {
				List<Employee> employees = employeeService.getRegistrations();
				model.addAttribute("employees",employees);
				return "list_registrations";//--> RequestDispatcher
			}
	
}

Step 3: Add method to fetch all data from db in service layer: EmployeeService.java
package com.app.service;

import java.util.List;

import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.app.dto.EmployeeDto;
import com.app.entity.Address;
import com.app.entity.Employee;
import com.app.repository.AddressRepository;
import com.app.repository.EmployeeRepository;

@Service
public class EmployeeService {
	
	@Autowired
	private EmployeeRepository employeeRepository;
	
	@Autowired
	private AddressRepository addressRepository;
	
	public void saveEmployeeDetails(EmployeeDto employeeDto) {
		Employee emp = new Employee();
		BeanUtils.copyProperties(employeeDto,emp);
		employeeRepository.save(emp);
		
		Address address = new Address();
		BeanUtils.copyProperties(employeeDto,address);
		addressRepository.save(address);
	}

	public void register(String firstName, String lastName, String email, String mobile) {
		Employee emp = new Employee();
		emp.setFirstName(firstName);
		emp.setLastName(lastName);
		emp.setEmail(email);
		emp.setMobile(mobile);
		employeeRepository.save(emp);
		
	}
	
	public List<Employee> getRegistrations() {
		List<Employee> employees = employeeRepository.findAll();
		return employees;
		
	}

}
Step 4: List all data in a table  - list_registrations.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ include file="menu.jsp" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Employee</title>
</head>
<body>
	<h2>View Registrations</h2>
	
	<table>
		<tr>
			<th>
				First name
			</th>
			<th>
				Last name
			</th>
			<th>
				Email
			</th>
			<th>
				Mobile
			</th>
			<th>
				Delete
			</th>
		</tr>
		
		<c:forEach var="emp" items="${employees}">
		<tr>
			<td>
				${emp.firstName}
			</td>
			<td>
				${emp.lastName}
			</td>
			<td>
				${emp.email}
			</td>
			<td>
				${emp.mobile}
			</td>
			
		</tr>
		
		</c:forEach>
		
	
	</table>
	
</body>
</html>

############################################
Delete a employee registration
##########################################

Step 1: Modify list_registrations.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ include file="menu.jsp" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Employee</title>
</head>
<body>
	<h2>View Registrations</h2>
	
	<table>
		<tr>
			<th>
				First name
			</th>
			<th>
				Last name
			</th>
			<th>
				Email
			</th>
			<th>
				Mobile
			</th>
			<th>
				Delete
			</th>
		</tr>
		
		<c:forEach var="emp" items="${employees}">
		<tr>
			<td>
				${emp.firstName}
			</td>
			<td>
				${emp.lastName}
			</td>
			<td>
				${emp.email}
			</td>
			<td>
				${emp.mobile}
			</td>
			<td>
				<a href="deleteRegistration?id=${emp.id}">delete</a>
			</td>
			
		</tr>
		
		</c:forEach>
		
	
	</table>
	
</body>
</html>

Step 2: Create handler method inside EmployeeController.java

			@RequestMapping("/deleteRegistration")
			public String deleteRegistrationById(@RequestParam long id, ModelMap model) {
				employeeService.deleteRegistrationById(id);
				
				List<Employee> employees = employeeService.getRegistrations();
				model.addAttribute("employees",employees);
				return "list_registrations";//--> RequestDispatcher
			}
Step 3: In service layer add the following method: EmployeeService.java
public void deleteRegistrationById(long id) {
		employeeRepository.deleteById(id);
	}

###############################
Update Employee Details
##############################
Step 1: Modify list_registrations.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ include file="menu.jsp" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Employee</title>
</head>
<body>
	<h2>View Registrations</h2>
	
	<table>
		<tr>
			<th>
				First name
			</th>
			<th>
				Last name
			</th>
			<th>
				Email
			</th>
			<th>
				Mobile
			</th>
			<th>
				Delete
			</th>
		</tr>
		
		<c:forEach var="emp" items="${employees}">
		<tr>
			<td>
				${emp.firstName}
			</td>
			<td>
				${emp.lastName}
			</td>
			<td>
				${emp.email}
			</td>
			<td>
				${emp.mobile}
			</td>
			<td>
				<a href="deleteRegistration?id=${emp.id}">delete</a>
			</td>
			<td>
				<a href="getRegistrationById?id=${emp.id}">update</a>
			</td>
		</tr>
		
		</c:forEach>
		
	
	</table>
	
</body>
</html>

Step 2: Modify EmployeeController.java

			@RequestMapping("/getRegistrationById")
			public String getRegistrationById(@RequestParam long id, ModelMap model) {
				Employee employee = employeeService.getRegistrationById(id);
				model.addAttribute("employee",employee);
				return "update_registration";//--> RequestDispatcher
			}
Step 3: Modify Service layer - EmployeeService.java
public Employee getRegistrationById(long id) {
		Optional<Employee> opEmp = employeeRepository.findById(id);
		return opEmp.get();
	}
Step 4: Create update_registration.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ include file="menu.jsp" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Register</title>
</head>
<body>
	<h2>Register here..</h2>
	<form action="updateReg" method="post">
		<pre>
		    <input type="hidden" name="id" value="${employee.id}"/>
			First Name <input type="text" name="firstName" value="${employee.firstName}"/>
			Last Name <input type="text" name="lastName" value="${employee.lastName}"/>
			Email Id <input type="text" name="email" value="${employee.email}"/>
			Mobile <input type="text" name="mobile" value="${employee.mobile}"/>
			<input type="submit" value="update"/>
		</pre>
	</form>
	${msg}
</body>
</html>











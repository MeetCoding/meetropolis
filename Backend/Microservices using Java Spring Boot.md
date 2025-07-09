A **microservice** is a way of building software applications by breaking them down into many small, independent pieces, where each piece does one specific job.

Microservices usually use RESTful API endpoints to communicate with each other via the internet.

We can use Java Spring Boot to create a RESTful API that serves to other microservices in our application.

# Structure of a Spring Boot Project for Microservices

A well-structured Spring Boot project is crucial for building scalable and maintainable microservices. Here’s a detailed, layman-friendly breakdown of how such a project is typically organized and why each part matters.

## 1. Project Root
At the top level, you’ll find files like:
- `pom.xml` or `build.gradle`: Build configuration (Maven or Gradle)
- `README.md`: Project documentation
- `.gitignore`: Files to ignore in version control
## 2. src/main/java

This is where all your Java source code lives. The package structure inside usually follows this pattern for each microservice:

```text
com/   
	yourcompany/    
		servicename/      
			controller/      
			service/      
			repository/      
			model/      
			dto/      
			config/      
			exception/      
			Application.java
```

**Explanation of Key Folders:**

- **controller/**: Contains REST controllers that define the API endpoints.
- **service/**: Holds business logic; services are called by controllers.
- **repository/**: Interfaces for database operations (often extending JpaRepository or similar).
- **model/**: Entity classes representing database tables.
- **dto/**: Data Transfer Objects, used to decouple API contracts from internal models.
- **config/**: Configuration classes (e.g., security, beans, external integrations).
- **exception/**: Custom exception classes and handlers.
- **Application.java**: The main class with the `@SpringBootApplication` annotation and `main()` method to launch the service
## 3. src/main/resource
This folder contains non-code resources:
- **application.properties** or **application.yml**: Main configuration file (e.g., server port, database URL, logging).
- **static/**: Static web resources (images, CSS, JavaScript).
- **templates/**: Server-side templates (if using Thymeleaf or similar).
- **messages.properties**: Localization files.
Spring Boot serves static content from `/static`, `/public`, `/resources`, or `/META-INF/resources` by default.
## 4. src/test/java
Mirrors the main Java package structure, but for tests:
- Unit and integration tests for controllers, services, and repositories.
## **5. Target or Build Folder**
- **target/** (Maven) or **build/** (Gradle): Generated files and compiled classes. You rarely edit anything here directly.

## **Example Folder Structure**
```
my-microservice/ 
│ 
├── src/ 
│   ├── main/ 
│   │   ├── java/ 
│   │   │   └── com/yourcompany/servicename/ 
│   │   │       ├── controller/ 
│   │   │       ├── service/ 
│   │   │       ├── repository/ 
│   │   │       ├── model/ 
│   │   │       ├── dto/ 
│   │   │       ├── config/ 
│   │   │       ├── exception/ 
│   │   │       └── Application.java 
│   │   └── resources/ 
│   │       ├── application.properties 
│   │       ├── static/ 
│   │       └── templates/ 
│   └── test/ 
│       └── java/ 
│           └── com/yourcompany/servicename/ 
│               └── (tests) 
├── pom.xml 
└── README.md
```

## Summary Table

|Folder/File|Purpose|
|---|---|
|controller/|API endpoints|
|service/|Business logic|
|repository/|Database access|
|model/|Entity/data model classes|
|dto/|Data transfer objects|
|config/|Configuration classes|
|exception/|Custom exceptions and handlers|
|Application.java|Main entry point of the microservice|
|resources/application.properties|Configuration (port, DB, etc.)|
|static/, templates/|Static files and server-side templates|
|test/|Unit and integration tests|
# Example Spring Boot Microservice

base package is `com.example.productservice`.
## 1. Product.java (Model/Entity)

```java
package com.example.productservice.model; 
import javax.persistence.*; 

@Entity 
public class Product {     
	@Id    
	@GeneratedValue(strategy = GenerationType.IDENTITY)    
	private Long id;     
	
	private String name;    
	private double price;     
	
	// Constructors    
	public Product() {}     
	public Product(String name, double price) {        
		this.name = name;        
		this.price = price;    
	}     
	
	// Getters and Setters    
	public Long getId() { return id; }    
	public void setId(Long id) { this.id = id; }     
	public String getName() { return name; }    
	public void setName(String name) { this.name = name; }     
	public double getPrice() { return price; }    
	public void setPrice(double price) { this.price = price; } 
}
```
## 2. ProductRepository.java (Repository)
```java
package com.example.productservice.repository; 
import com.example.productservice.model.Product; 
import org.springframework.data.jpa.repository.JpaRepository; 

// JpaRepository provides CRUD methods out of the box 
public interface ProductRepository extends JpaRepository<Product, Long> {     
	// You can add custom query methods here if needed 
}
```
## 3. ProductService.java (Service)

```java
package com.example.productservice.service; 
import com.example.productservice.model.Product; 
import com.example.productservice.repository.ProductRepository; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.stereotype.Service; 
import java.util.List; 
import java.util.Optional; 

@Service 
public class ProductService {     
	
	@Autowired    
	private ProductRepository repository;     
	
	public List<Product> getAllProducts() {        
		return repository.findAll();    
	}     
	public Optional<Product> getProductById(Long id) {        
	return repository.findById(id);    
	}     
	public Product createProduct(Product product) {        
		return repository.save(product);    
	}     
	public Optional<Product> updateProduct(Long id, Product productDetails) {        return repository.findById(id).map(product -> {            
			product.setName(productDetails.getName());            
			product.setPrice(productDetails.getPrice());            
			return repository.save(product);        
		});    
	}     
	public void deleteProduct(Long id) {        
		repository.deleteById(id);    
	} 
}
```

## 4. ProductController.java (Controller)

```java
package com.example.productservice.controller; 
import com.example.productservice.model.Product; 
import com.example.productservice.service.ProductService; 
import org.springframework.beans.factory.annotation.Autowired; 
import org.springframework.http.ResponseEntity; 
import org.springframework.web.bind.annotation.*; 
import java.util.List; 

@RestController 
@RequestMapping("/api/products") 
public class ProductController {     

	@Autowired    
	private ProductService service;     
	
	@GetMapping    
	public List<Product> getAllProducts() {        
		return service.getAllProducts();    
	}     
	
	@GetMapping("/{id}")    
	public ResponseEntity<Product> getProductById(@PathVariable Long id) {    
		return service.getProductById(id)
			.map(ResponseEntity::ok)                
			.orElse(ResponseEntity.notFound().build());    
	}     
	
	@PostMapping    
	public Product createProduct(@RequestBody Product product) {        
		return service.createProduct(product);    
	}     
	
	@PutMapping("/{id}")    
	public ResponseEntity<Product> updateProduct(@PathVariable Long id, @RequestBody Product product) {        
		return service.updateProduct(id, product)
			.map(ResponseEntity::ok)                
			.orElse(ResponseEntity.notFound().build());    
	}     
	
	@DeleteMapping("/{id}")    
	public ResponseEntity<Void> deleteProduct(@PathVariable Long id) {        
		service.deleteProduct(id);        
		return ResponseEntity.noContent().build();    
	} 
}
```
## 5. Application.java (Main Class)

```java
package com.example.productservice; 
import org.springframework.boot.SpringApplication; 
import org.springframework.boot.autoconfigure.SpringBootApplication; 

@SpringBootApplication 
public class Application {     
	public static void main(String[] args) {        
		SpringApplication.run(Application.class, args);    
	} 
}
```
## 6. application.properties (Configuration)

```text
# Use H2in-memory database for demo purposes spring.datasource.url=jdbc:h2:mem:testdb spring.datasource.driverClassName=org.h2.Driver spring.datasource.username=sa spring.datasource.password= spring.jpa.database-platform=org.hibernate.dialect.H2Dialect # Show SQL in the console spring.jpa.show-sql=true # Automatically create/drop tables based on entities spring.jpa.hibernate.ddl-auto=update # Run server on port 8080 server.port=8080
```

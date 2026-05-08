Spring Cloud API gateway instead of ZUUL API gateway because Zuul is in maintenance mode/not support/Deprecated
To Route the HTTP request to the destination microservices, we use API gateway.
Spring Cloud API Gateway is preferred over Zuul because Zuul is deprecated and in maintenance mode.

API Gateway is used to route HTTP requests to different microservices. 
All microservices register themselves with Eureka Discovery Server, which stores their addresses.

When multiple instances of a microservice are running, API Gateway performs load balancing and distributes requests among available instances. This improves scalability, performance, and reliability.

Spring Cloud API Gateway acts as a central entry point for all client requests. 
It supports routing, filtering, authentication, logging, and request/response modification.
Filters can be:
• Pre Filters – executed before forwarding the request
• Post Filters – executed after receiving the response

Custom filters can also validate JWT tokens and provide security before allowing requests to access microservices.
 


 
 
 
1.	Create the spring project (ApiGateway)-add following dependency 1) Gateway 2)Spring Reactive Web 3) Eureka Discovery Client- 
 <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.4.5</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
 <properties>
		<java.version>21</java.version>
		<spring-cloud.version>2024.0.1</spring-cloud.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-gateway</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>

2.	Open Application.properties file and add below properties 
 spring.application.name=api-gateway
server.port=8082
eureka.client.service-url.defaultZone=http://localhost:8010/eureka
spring.cloud.gateway.discovery.locator.enabled=true


3.	Now run the “PhotoAppDiscoveryService” it is discovery server
4.	Now Run PhotoAppUsers microservice
5.	Run ApiGateway 
6.	Go to Eureka dashboard(Eureka)- you will see below image
 
7.	Now go to postam and type http://localhost:8082/USERS-WS/users/status/check
Note-choose the GET request in postman(below is image for working the project)
 

The main purpose of an API Gateway in industry is to act as a single entry point for all microservices.
Instead of clients calling many services directly, everything goes through the API Gateway.
Industry Purpose of API Gateway
1. Single Entry Point
Without Gateway
Frontend → User Service
Frontend → Order Service
Frontend → Payment Service

With Gateway: Frontend → API Gateway → Microservices

2. Dynamic Routing
Gateway automatically routes requests using Eureka.
Example:
/users/**  → users-ws
/orders/** → orders-ws	3. 
            Load Balancing
If multiple instances exist:
users-ws : 8081
users-ws : 8083
users-ws : 8085
Gateway distributes traffic automatically.
Used in:
•	Amazon 
•	Netflix 
•	Flipkart 
•	Banking systems
Security Centralization
Authentication and authorization handled at one place.
Industry uses Gateway for:
•	JWT validation 
•	OAuth2 
•	API keys 
•	Role-based access	•	Hiding Internal Services
•	Clients never see actual microservice ports.
•	Example:
•	Client sees:
•	/api/users
•	but internally:
•	localhost:8081
localhost:8082

   6. Logging & Monitoring
Gateway tracks:
•	Request logs 
•	Response time 
•	Errors 
•	Traffic analytics 
Very important in production
	  7. Rate Limiting
Protects backend from overload.
Example:
•	only 100 requests/minute allowed 
Used in:
•	Payment APIs 
•	Banking APIs 
•	Public APIs

8. Fault Tolerance
Gateway can:
•	retry failed requests 
•	fallback responses 
•	circuit breaker support 
Helps system survive partial failures.
Real Industry Flow
   
Why Companies Use This
Because modern systems have:
•	50+ 
•	100+ 
•	even 1000+ microservices 
Without Gateway, management becomes impossible.
These are used heavily in:
•	Java Backend Development 
•	Cloud Projects 
•	Enterprise Applications 
•	Banking 
•	E-commerce 
•	OTT platforms 
•	FinTech



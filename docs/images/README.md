# Spring Cloud Gateway — Microservices Demo

A hands-on demo project for understanding **Spring Cloud Gateway** with **Netflix Eureka Service Discovery**.  
Contains **3 Spring Boot microservices** that work together as a complete system.

> 📚 Created for lecture demonstration — Step-by-step guide available in `docs/` folder.

---

## 📦 Projects Overview

| Project | Port | Role |
|---|---|---|
| `PhotoAppDiscoveryService` | `8010` | Eureka Server — Service Registry |
| `ApiGateway` | `8082` | Spring Cloud Gateway — Single Entry Point |
| `PhotoAppUsers` | `random (0)` | Microservice — Users REST API |

---

## 🏗️ Architecture

```
Client (Browser / Postman)
           ↓
     API Gateway (:8082)
           ↓
  Eureka Discovery (:8010)
       ↓         ↓
 PhotoAppUsers  (other services...)
   (:random)
```

- The **Eureka Server** acts as a service registry — all services register here on startup.
- The **API Gateway** uses Eureka to discover services dynamically and route requests.
- **PhotoAppUsers** registers itself with a random port (`server.port=0`) — Eureka handles the actual address.
- The client **never needs to know** the actual port of any microservice — the Gateway handles everything.

---

## 🚀 How to Run

> ⚠️ **Start services in this exact order!**

### Step 1 — Start Eureka Discovery Server
```bash
cd PhotoAppDiscoveryService
./mvnw spring-boot:run
```
✅ Open Eureka Dashboard → http://localhost:8010

---

### Step 2 — Start API Gateway
```bash
cd ApiGateway
./mvnw spring-boot:run
```
✅ Gateway running → http://localhost:8082

---

### Step 3 — Start Users Microservice
```bash
cd PhotoAppUsers
./mvnw spring-boot:run
```
✅ Registers itself automatically with Eureka on a random port

---

## 📸 Project Running Screenshots

### Eureka Dashboard — Services Registered
> Both **API-GATEWAY** and **USERS-WS** are UP and registered with Eureka

![Eureka Dashboard](docs/images/eureka-dashboard.png)

---

### Postman — API Test via Gateway
> Hitting the Users service **through the Gateway** returns `Working` with **200 OK**

![Postman Test](docs/images/postman-image.png)

---

## 🧪 Test the Gateway

Once all 3 services are running, test the Users service **through the Gateway**:

```
GET http://localhost:8082/USERS-WS/users/status/check
```

✅ Expected Response:
```
Working
```

> The Gateway routes the request to the Users service via Eureka — you never need to know the actual port of PhotoAppUsers!

---

## 🛠️ Tech Stack

| Technology | Version |
|---|---|
| Java (PhotoAppUsers, ApiGateway) | 21 |
| Java (PhotoAppDiscoveryService) | 17 |
| Spring Boot | 4.0.6 |
| Spring Cloud | 2025.1.1 |
| Spring Cloud Gateway | Reactive (non-blocking) |
| Netflix Eureka | Service Registry & Discovery |

---

## 📁 Project Structure

```
spring-boot-cloud-gateway/
│
├── PhotoAppDiscoveryService/        ← Eureka Server
│   └── src/main/java/com/example/
│       └── PhotoAppDiscoveryServiceApplication.java
│
├── ApiGateway/                      ← Spring Cloud Gateway
│   └── src/main/java/com/photoapp/api/gateway/
│       └── ApiGatewayApplication.java
│
├── PhotoAppUsers/                   ← Users Microservice
│   └── src/main/java/com/example/
│       ├── PhotoAppUsersApplication.java
│       └── ui/controller/UsersController.java
│
├── docs/
│   ├── images/
│   │   ├── eureka-dashboard.png     ← Eureka Dashboard screenshot
│   │   └── postman-image.png        ← Postman test screenshot
│   └── 03-SpringAPI-gateway.docx   ← Step-by-step lecture notes
│
└── README.md
```

---

## 💡 Key Concepts (For Lecture Reference)

| Concept | Where to See It |
|---|---|
| `@EnableEurekaServer` | `PhotoAppDiscoveryServiceApplication.java` |
| Eureka Client config | `application.properties` in ApiGateway & PhotoAppUsers |
| Gateway discovery locator | `spring.cloud.gateway.discovery.locator.enabled=true` |
| Random port registration | `server.port=0` in PhotoAppUsers |
| Service routing via Gateway | `http://localhost:8082/USERS-WS/...` |

---

## 📝 How Gateway Routes Work

```
Request → http://localhost:8082/USERS-WS/users/status/check
                    ↓
         Gateway sees: /USERS-WS/
                    ↓
         Looks up USERS-WS in Eureka
                    ↓
         Forwards to → http://localhost:{random-port}/users/status/check
```

---

## 📖 Lecture Notes

Step-by-step creation guide is available in the `docs/` folder:
- 📄 `docs/03-SpringAPI-gateway.docx`

---

## 👨‍💻 Author

**Sumedh Manwatkar**  
Assistant Professor — CSE Department, GLA University  
Java Full Stack Trainer

---

*⭐ If this helped you, give it a star on GitHub!*

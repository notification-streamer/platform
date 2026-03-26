# 🚀 Notification Streamer

A **distributed, event-driven notification platform** built using **Spring Boot and Apache Kafka** to deliver scalable, reliable, and multi-channel notifications (Email, SMS, WhatsApp, Push).

---

## 📌 Overview

Notification Streamer is designed to handle **high-throughput notification delivery** using a **microservices architecture**. It ensures fault tolerance, scalability, and flexibility through asynchronous communication and orchestration.

---

## 🏗️ Architecture

![Architecture Diagram](./assets/notification-streamer-hld.png)

### 🔹 Key Components

* **Entry Service** – Validates requests, ensures idempotency, and acts as API entry point
* **Orchestrator Service** – Central workflow engine coordinating all services
* **Rule Service** – Applies business rules and user preferences (Drools-based)
* **Template Service** – Fetches templates and formats messages dynamically
* **Channel Services** – Email, SMS, WhatsApp notification handlers
* **Log Service** – Handles logging, auditing, and analytics

---

## 🔄 Event Flow

1. Client sends API request → Entry Service
2. Entry Service publishes event → `notification.streamer.orchestrator.svc.entry.request.v1`
3. Orchestrator triggers Rule Service → `rule.svc.request`
4. Rule Service evaluates rules → sends response → `rule.svc.response`
5. Orchestrator calls Template Service → `template.svc.request`
6. Template Service formats message → sends response → `template.svc.response`
7. Orchestrator sends final event → Channel Services (Email/SMS/WhatsApp)
8. Notifications are delivered via external providers (SMTP, Twilio, WhatsApp API)
9. Logs are published to Elasticsearch

---

## 📡 Kafka Topics

| Topic                                                   | Description       |
| ------------------------------------------------------- | ----------------- |
| notification.streamer.orchestrator.svc.entry.request.v1 | Entry request     |
| notification.streamer.rule.svc.request.v1               | Rule request      |
| notification.streamer.rule.svc.response.v1              | Rule response     |
| notification.streamer.template.svc.format.request.v1    | Template request  |
| notification.streamer.template.svc.format.response.v1   | Template response |
| notification.streamer.email.svc.request.v1              | Email trigger     |
| notification.streamer.sms.svc.request.v1                | SMS trigger       |
| notification.streamer.whatsapp.svc.request.v1           | WhatsApp trigger  |
| notification.retries                                    | Retry topic       |
| notification.DLQ                                        | Dead Letter Queue |

---

## ⚙️ Tech Stack

* **Backend**: Spring Boot
* **Messaging**: Apache Kafka
* **Databases**: MongoDB, MySQL, Redis
* **Rule Engine**: SpEL (Spring Expression Language)
* **Search & Logging**: Elasticsearch, Kibana
* **Monitoring**: Prometheus, Grafana
* **Containerization**: Docker

---

## 🔥 Key Features

* ⚡ Event-driven microservices architecture
* 🔁 Retry mechanism with exponential backoff
* ☠️ Dead Letter Queue (DLQ) for failure handling
* ♻️ Idempotent consumers to avoid duplicates
* 🎯 Dynamic rule-based channel selection
* 📊 Centralized logging and observability
* 🚀 Stateless services for horizontal scalability

---

## 🔁 Retry Mechanism

* Transient failures → Routed to **Retry Topic** with backoff
* Permanent failures → Moved to **DLQ**
* Ensures **no data loss and high reliability**

---

## 🐳 Deployment

Run using Docker:

```bash
docker-compose up --build
```

---

## 📂 Project Structure

```
notification-streamer/
 ├── entry-service/
 ├── orchestrator-service/
 ├── rule-service/
 ├── template-service/
 ├── email-service/
 ├── sms-service/
 ├── whatsapp-service/
 ├── log-service/
 ├── kafka/
 ├── docker-compose.yml
```

---

## 🚀 Future Enhancements

* Priority-based queues (OTP vs marketing)
* Rate limiting & throttling
* Multi-region deployment
* Notification personalization

---

## 👨‍💻 Author

**Sundar Pirabu Raj R**
Backend Developer | Java | Distributed Systems

---

## ⭐ Contributing

Feel free to fork, raise issues, and contribute.

---

## 📜 License

MIT License

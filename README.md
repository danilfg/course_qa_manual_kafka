# 🚀 Apache Kafka Cluster with REST Proxy and Kafka UI

![Kafka](https://img.shields.io/badge/Apache-Kafka-black?logo=apachekafka)
![Docker](https://img.shields.io/badge/Docker-compose-blue?logo=docker)
![Kafka UI](https://img.shields.io/badge/Kafka-UI-green)
![REST Proxy](https://img.shields.io/badge/Kafka-REST%20Proxy-orange)

This repository contains a **ready-to-run Apache Kafka cluster** designed for **learning and practicing message-based systems**.

The environment includes:

* Apache Kafka cluster
* Zookeeper
* Kafka REST Proxy
* Kafka UI

It allows you to **experiment with Kafka, send and consume messages, and explore streaming architectures** in a simple Docker environment.

---

# 👨‍🏫 Author

This project is part of educational materials created by:

**Daniil Nikolaev**
Mentor in **Manual and Automation Testing**

Telegram:
[https://t.me/aqa_pro_mentor](https://t.me/aqa_pro_mentor)

---

# 🎓 About This Project

This setup is commonly used in **QA Automation and backend testing training**.

It helps students learn how to work with:

* event-driven architectures
* Kafka messaging
* REST interaction with Kafka
* distributed systems

Students can use this environment to practice:

* producing messages
* consuming messages
* testing event pipelines
* debugging message flows

---

# 🏗 Architecture

This `docker-compose.yml` file starts a **distributed Kafka environment** consisting of:

| Service          | Purpose                               |
| ---------------- | ------------------------------------- |
| Zookeeper        | Coordinates Kafka brokers             |
| Kafka Broker 1   | Handles message storage and streaming |
| Kafka Broker 2   | Provides high availability            |
| Kafka REST Proxy | Allows HTTP interaction with Kafka    |
| Kafka UI         | Web interface for managing Kafka      |

---

# ⚙️ Services Overview

## Zookeeper

Purpose:

Coordinates Kafka brokers and maintains cluster metadata.

Image:

```
confluentinc/cp-zookeeper:7.5.0
```

Port:

```
2181
```

---

## Kafka Brokers

Purpose:

Store and process messages.

Two brokers are included in this setup.

| Broker | Port |
| ------ | ---- |
| kafka1 | 9092 |
| kafka2 | 9093 |

Image:

```
confluentinc/cp-kafka:7.5.0
```

This allows students to experiment with **multi-broker Kafka clusters**.

---

## Kafka REST Proxy

Purpose:

Provides an **HTTP API for interacting with Kafka**.

Image:

```
confluentinc/cp-kafka-rest:7.5.0
```

Port:

```
8082
```

This allows sending and receiving Kafka messages using:

* curl
* Postman
* automated tests

---

## Kafka UI

Purpose:

A graphical interface for working with Kafka clusters.

Image:

```
provectuslabs/kafka-ui
```

Port:

```
8080
```

Kafka UI allows you to:

* view topics
* inspect messages
* manage consumer groups
* monitor cluster status

---

# 🚀 Getting Started

## Step 1 — Start the Kafka cluster

Run:

```bash
docker compose up -d
```

This will start all services in the background.

---

## Step 2 — Open Kafka UI

Open your browser and navigate to:

```
http://localhost:8080
```

You will see the Kafka cluster dashboard.

---

# 🧵 Create a Topic

## Using Kafka UI

1. Open **Topics**
2. Click **Add Topic**
3. Fill parameters:

| Field              | Example    |
| ------------------ | ---------- |
| Topic name         | `my_topic` |
| Partitions         | `1`        |
| Replication factor | `1`        |

Click **Create**.

---

## Using CLI

You can also create a topic using the Kafka CLI:

```bash
docker exec -it kafka1 kafka-topics \
--create \
--topic my_topic \
--bootstrap-server kafka1:29092 \
--replication-factor 1 \
--partitions 1
```

---

# 📡 Send Messages via REST Proxy

You can send messages to Kafka using **curl, Postman or automated tests**.

Example:

```bash
curl -X POST \
-H "Content-Type: application/vnd.kafka.json.v2+json" \
--data '{
  "records":[
    {
      "value":{
        "message":"Hello Kafka"
      }
    }
  ]
}' \
http://localhost:8082/topics/my_topic
```

---

# 📥 Consume Messages

Example request to read messages:

```bash
curl -X GET \
-H "Accept: application/vnd.kafka.json.v2+json" \
http://localhost:8082/consumers/my-consumer-group/instances/my-consumer/records
```

---

# 📈 Scaling the Cluster (Optional)

You can expand the cluster by adding more Kafka brokers.

Simply duplicate the broker configuration in `docker-compose.yml` and adjust:

* broker id
* ports
* volumes

---

# 🌐 Networking

All services run inside a Docker network:

```yaml
networks:
  kafka-net:
    driver: bridge
```

This allows containers to communicate internally.

---

# 💾 Data Persistence

Each Kafka broker uses Docker volumes:

```yaml
volumes:
  kafka1_data:
  kafka2_data:
```

This ensures that messages are **not lost when containers restart**.

---

# 🎓 Learning Scenarios

This Kafka environment can be used for learning:

### Event-Driven Testing

Simulating services that communicate through Kafka events.

### API Testing

Testing REST Proxy endpoints.

### Automation Testing

Using tools like:

* Pytest
* Postman
* k6
* custom consumers

### Observability

Analyzing message flow between services.

---

# 📜 License

This project is distributed under the **MIT License**.

---

# ⭐ If This Project Helps You

Consider starring the repository ⭐.

It helps other engineers discover this Kafka learning environment.

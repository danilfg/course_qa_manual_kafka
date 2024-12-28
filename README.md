# 🚀 Кластер Apache Kafka с REST Proxy и Kafka UI

### **Простое развертывание полностью функционального окружения Apache Kafka с REST Proxy и Kafka UI для удобного взаимодействия и управления.**

Этот файл `docker-compose.yml` позволяет развернуть **распределённый кластер Kafka**, включающий:
- **Zookeeper** для координации брокеров.
- **Несколько брокеров Kafka** для обеспечения высокой доступности и отказоустойчивости.
- **REST Proxy** для взаимодействия с Kafka через HTTP API.
- **Kafka UI** для удобного графического интерфейса управления.

Независимо от вашего уровня опыта, эта настройка упростит работу с Kafka, позволив сосредоточиться на создании мощных приложений для обработки потоковых данных.

---

# 🚀 Apache Kafka Cluster with REST Proxy and Kafka UI

### **Easily set up a fully functional Apache Kafka environment with REST Proxy and Kafka UI for seamless interaction and management.**

This `docker-compose.yml` file allows you to spin up a **distributed Kafka cluster** that includes:
- **Zookeeper** for broker coordination.
- **Multiple Kafka brokers** for high availability and fault tolerance.
- **REST Proxy** for HTTP-based interaction with Kafka.
- **Kafka UI** for an intuitive graphical interface to monitor and manage Kafka clusters.

Whether you're a beginner or an experienced developer, this setup simplifies working with Kafka, letting you focus on building robust, real-time streaming applications.

---

## **Основные возможности / Key Features**

- ✅ **Распределённый кластер Kafka / Distributed Kafka Cluster**: Легко масштабируемый с несколькими брокерами.
- ✅ **REST Proxy**: Обеспечивает взаимодействие с Kafka через HTTP.
- ✅ **Kafka UI**: Интуитивно понятный веб-интерфейс для управления топиками, брокерами и консюмерами.
- ✅ **Сохранение данных / Persistent Data**: Использует Docker тома для хранения данных Kafka.

---

## **Услуги / Services Overview**

### 1. **Zookeeper**
- **Назначение / Purpose**: Управляет и координирует брокеров Kafka.
- **Image**: `confluentinc/cp-zookeeper:7.5.0`
- **Порт / Port**: `2181` (порт клиента Zookeeper).

### 2. **Kafka Brokers**
- **Назначение / Purpose**: Обрабатывают и хранят сообщения.
- **Брокеры в настройке / Brokers in this setup**:
  - **kafka1**: Доступен по порту `9092` / Accessible via port `9092`.
  - **kafka2**: Доступен по порту `9093` / Accessible via port `9093`.
- **Image**: `confluentinc/cp-kafka:7.5.0`.

### 3. **REST Proxy**
- **Назначение / Purpose**: Упрощает взаимодействие с Kafka через HTTP API.
- **Image**: `confluentinc/cp-kafka-rest:7.5.0`
- **Порт / Port**: `8082`.

### 4. **Kafka UI**
- **Назначение / Purpose**: Предоставляет удобный интерфейс для работы с кластером Kafka.
- **Image**: `provectuslabs/kafka-ui:latest`
- **Порт / Port**: `8080`.

---

## **Как использовать / How to Use**

### **Шаг 1 / Step 1**: Запустите кластер Kafka / Start the Kafka Cluster
```bash
docker-compose up -d
```

### **Шаг 2 / Step 2**: Доступ к Kafka UI / Access Kafka UI
Откройте браузер и перейдите по адресу / Open your browser and go to:
```
http://localhost:8080
```

### **Шаг 3 / Step 3**: Добавьте топик / Add a Topic

#### Через Kafka UI / Using Kafka UI:
1. Перейдите в раздел **Topics** в интерфейсе.
2. Нажмите кнопку **Add a Topic**.
3. Заполните параметры:
   - **Имя топика / Topic Name**: Например, `my_topic`.
   - **Количество разделов / Number of Partitions**: Например, `1`.
   - **Коэффициент репликации / Replication Factor**: Например, `1`.
4. Нажмите **Create**.

#### Через REST Proxy / Using REST Proxy:
Выполните следующий запрос с помощью `curl`:
```bash
curl -X POST \
  -H "Content-Type: application/vnd.kafka.json.v2+json" \
  --data '{
    "records": []
  }' \
  http://localhost:8082/topics/my_topic
```

#### Через CLI / Using CLI:
Если вы предпочитаете использовать CLI, выполните следующую команду:
```bash
docker exec -it kafka1 kafka-topics --create --topic my_topic --bootstrap-server kafka1:29092 --replication-factor 1 --partitions 1
```

---

### **Шаг 4 / Step 4**: Взаимодействие через REST Proxy / Interact via REST Proxy
Используйте **curl**, **Postman**, или любой HTTP-клиент для работы с Kafka.  
- Пример: Отправка сообщения в топик / Example: Sending a message to a topic:
  ```bash
  curl -X POST \
    -H "Content-Type: application/vnd.kafka.json.v2+json" \
    --data '{
      "records": [
        {
          "value": {
            "message": "Hello, Kafka!"
          }
        }
      ]
    }' \
    http://localhost:8082/topics/my_topic
  ```

---

### **Шаг 5 / Step 5**: Масштабирование (опционально) / Scale Up (Optional)
Добавьте больше брокеров в кластер, дублируя и изменяя конфигурации брокеров в `docker-compose.yml`.

---

## **Сеть / Networking**

Все сервисы подключены через Docker-сеть `kafka-net` / All services are connected via the `kafka-net` Docker network:
```yaml
networks:
  kafka-net:
    driver: bridge
```

---

## **Тома / Volumes**

Каждый брокер Kafka использует том для сохранения данных / Each Kafka broker uses a volume to persist data:
```yaml
volumes:
  kafka1_data:
  kafka2_data:
```

---

## **Лицензия / License**

Этот файл предоставляется под лицензией MIT / This configuration is distributed under the MIT License.

---

Наслаждайтесь удобной настройкой и работой с Kafka! 🎉 / Enjoy your seamless Kafka setup! 🎉

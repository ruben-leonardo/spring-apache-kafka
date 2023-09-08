# Spring for Apache Kafka Demo
Apache Kafka is a distributed and fault-tolerant stream processing system.

## Setup Kafka using Docker Compose
Docker Compose tool is handy for managing service setups. The Kafka cluster requires Zookeeper and Kafka Brokers.

```
version: '3'
services:
  zookeper:
    image: wurstmeister/zookeeper
    container_name: zookeeper
    ports: 
      - "2181:2181"
    networks:
      - kafka-net

  kafka:
    image: wurstmeister/kafka
    container_name: kafka
    ports:
      - "9092:9092"
    environment:
      KAFKA_ADVERTISED_LISTENERS: INSIDE://kafka:9092,OUTSIDE://localhost:9093
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: INSIDE:PLAINTEXT,OUTSIDE:PLAINTEXT
      KAFKA_LISTENERS: INSIDE://0.0.0.0:9092,OUTSIDE://0.0.0.0:9093
      KAFKA_INTER_BROKER_LISTENER_NAME: INSIDE
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_CREATE_TOPICS: "artisancodes:1:1"
    networks:
      - kafka-net
networks:
  kafka-net:
    driver: bridge
```

## Start the Kafka Cluster
```
docker-compose up -d
```
The above command will simply start the Zookeeper and Kafka containers in detached mode. The -d mode ensures that the
container runs in the background.

## Verify the Kafka Cluster
In order to verify that the Kafka Cluster is running successfully, run the following command to see the running
containers:
```
docker-compose ps
```
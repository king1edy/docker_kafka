## Apache Kafka on docker

# Getting Started with Apache Kafka on Docker: A Step-by-Step Guide

    Apache Kafka, a distributed streaming platform, is a powerful tool for building real-time data pipelines and streaming applications. Docker simplifies the process of setting up Kafka locally, allowing developers to experiment and develop with ease. In this article, we’ll walk through the steps to download, install, and run Apache Kafka using Docker, providing a hands-on implementation for beginners.

# Prerequisites:

    Before we begin, ensure that you have Docker installed on your local machine. You can download and install Docker from the official Docker website.

## Step 1: Create a Docker Compose File
    Create a docker-compose.yml file in a new directory to define the Kafka and Zookeeper services. Copy and paste the following content:


``

    version: '3'
    services:
    zookeeper:
        image: wurstmeister/zookeeper:latest
        ports:
        - "2181:2181"

    kafka:
        image: wurstmeister/kafka:latest
        ports:
        - "9092:9092"
        expose:
        - "9093"
        environment:
        KAFKA_ADVERTISED_LISTENERS: INSIDE://kafka:9093,OUTSIDE://localhost:9092
        KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: INSIDE:PLAINTEXT,OUTSIDE:PLAINTEXT
        KAFKA_LISTENERS: INSIDE://0.0.0.0:9093,OUTSIDE://0.0.0.0:9092
        KAFKA_INTER_BROKER_LISTENER_NAME: INSIDE
        KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
        KAFKA_CREATE_TOPICS: "my-topic:1:1"
        volumes:
    - /var/run/docker.sock:/var/run/docker.sock
``


    This docker-compose.yml file defines two services: zookeeper and kafka. The Kafka service is configured to expose ports 9092 and 9093.

## Step 2: Run Docker Compose
    Open a terminal in the directory where the docker-compose.yml file is located and run the following command to start the Kafka and Zookeeper containers:

``docker-compose up -d``

    This command will download the required Docker images and start the Kafka and Zookeeper services in detached mode (-d).

## Step 3: Verify Kafka Container is Running

    Check if the Kafka container is running by executing the following command:

`docker ps`

    You should see containers for both Kafka and Zookeeper in the list.

## Step 4: Create a Kafka Topic
    Create a Kafka topic using the following command:

`docker exec -it <kafka-container-id> /opt/kafka/bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic my-topic`

    Replace <kafka-container-id> with the actual container ID of the Kafka container (you can find it using docker ps).

## Step 5: Produce and Consume Messages
    Use the Kafka console producer and consumer to test your Kafka setup:

# Produce messages:

`docker exec -it <kafka-container-id> /opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic my-topic`

# Consume messages:

`docker exec -it <kafka-container-id> /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning`

    Replace <kafka-container-id> with the actual container ID of the Kafka container.

## Step 6: Stop and Remove Containers
    To stop and remove the Kafka and Zookeeper containers, run:

`docker-compose down`


    This will stop and remove the containers created by the docker-compose up command.

## Conclusion:

    Congratulations! You’ve successfully set up and run Apache Kafka on Docker locally. This hands-on guide provides a simple yet powerful environment for experimenting  with Kafka. As you continue to explore Kafka, consider integrating it into your applications to harness its capabilities for building real-time data pipelines and  streaming applications.

# Happy coding 🚀
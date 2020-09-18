# Kafka Spark Product Recommendation Engine

Python script with spark streaming to get product Id from one kafka producer, find relevant products via spark streaming and publish to another topic (kafka consumer).


## Data Files (Random data for the script)

- item-data.csv - Has Item Id and Name (~3,500 entries)
- user-item.csv - User:Item Id mapped on timestamp for matrix for collaborative filtering (~10,000 entries)


## Dependencies

- Zookeeper
- Kafka
- Spark: 2.4
- Spark Stream Package: Necessary package it is part of the code.
- Python: 3.7 (for Spark 2.4 version)


## How to run?

- **Zookeeper**: zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties <br/>
(Zoopkeeper is the broker that helps coordinate the show)

- **Kafka Server**: kafka-server-start /usr/local/etc/kafka/server.properties <br/>
(Kafka server syncs data flow between Producer and Consumers)

- **Register 1st Topic**: kafka-topics --create --zookeeper localhost:2181 --topic idpushtopic --partitions 1 --replication-factor 1 <br/>
(idpushtopic is the topic to get an Id of product for recommendations)

- **Register 2nd Topic**: kafka-topics --create --zookeeper localhost:2181 --topic prodRecommSend --partitions 1 --replication-factor 1 <br/>
(prodRecommSend is the topic to which the stream pushes the recommendations)

- **Producer**: kafka-console-producer --broker-list localhost:9092 --topic idpushtopic <br/>
(Producer to be opened in one window that ultimately pushes the product id to the spark stream)

- **Spark Streaming**: spark-submit --jars spark-streaming-kafka-0-8-assembly_2.11-2.4.7.jar kafka-product-recom.py localhost:2181 idpushtopic <br/>
(Stream helps fetch the Id, run the product recommendation and then acts as producer and pushes to another topic)

- **Consumer**: kafka-console-consumer --bootstrap-server localhost:9092 --topic prodRecommSend --from-beginning <br/>
(Consumer to be opened in one window that ultimately fetches the product list from the spark stream)


## How to test?

- Start Producer 1 and push an Id: 19444 (to test) <br/>

<img src="https://i.ibb.co/4wbKR14/Screenshot-2020-09-19-at-2-25-03-AM.png" alt="Screenshot-2020-09-19-at-2-25-03-AM" border="0">

- You'd see things happening on Spark streaming window. <br/>

<img src="https://i.ibb.co/Rpr4XwD/Screenshot-2020-09-19-at-2-23-38-AM.png" alt="Screenshot-2020-09-19-at-2-23-38-AM" border="0">

- You'd get the data in the consumer window <br/>

<img src="https://i.ibb.co/Qrck5ts/Screenshot-2020-09-19-at-2-21-15-AM.png" alt="Screenshot-2020-09-19-at-2-21-15-AM" border="0">



# Ubuntu Kafka
Ubuntu systemd support with confluent kafka installed

Confluent 
- Zookeeper
- Kafka broker
- Schema registry
- Kafka connect

# Installation

- create volume
```bash
docker volume create ubuntu-kafka-volume
```
- run container

```bash
docker run -d --name ubuntu-kafka -v ubuntu-kafka-volume:/opt/confluent/share/java -p 9092:9092 -p 8081:8081-p 8083:8083 -p 2181:2181 dhohirp/ubuntu-kafka:latest 
```
## Create Topic
```bash
docker exec -it ubuntu-kafka /opt/confluent/bin/kafka-avro-console-producer --topic test \
    --broker-list localhost:9092 \
    --property value.schema='{"type":"record","name":"testrecord","fields":[{"name":"name","type":"string"}]}'
```
## Topics List
```bash
docker exec -it systemd-ubuntu /opt/confluent/bin/kafka-topics --list --bootstrap-server localhost:9092
```
## Consume Topic

```bash
docker exec -it ubuntu-kafka /opt/confluent/bin/kafka-avro-console-consumer --topic test-tabletest --bootstrap-server localhost:9092 --from-beginning
```

## Plugin Location
```
/opt/confluent/share/java
```
Or you can also access on mounted docker volume directory

docker registry
https://hub.docker.com/repository/docker/dhohirp/ubuntu-kafka/general
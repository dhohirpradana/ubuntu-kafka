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
docker run -d --privileged -v /sys/fs/cgroup:/sys/fs/cgroup:ro --name ubuntu-kafka -v ubuntu-kafka-volume:/opt/confluent/share/java -p 9092:9092 -p 8081:8081 -p 8083:8083 -p 2181:2181 dhohirp/ubuntu-kafka:1.0 
```
## Services
- enable service
```bash
docker exec -it ubuntu-kafka systemctl enable zookeeper.service
```
```bash
docker exec -it ubuntu-kafka systemctl enable kafka.service
```
```bash
docker exec -it ubuntu-kafka systemctl enable schema-registry.service
```
```bash
docker exec -it ubuntu-kafka systemctl enable kafka-connect.service
```
- start service
```bash
docker exec -it ubuntu-kafka systemctl start zookeeper.service
```
```bash
docker exec -it ubuntu-kafka systemctl start kafka.service
```
```bash
docker exec -it ubuntu-kafka systemctl start schema-registry.service
```
```bash
docker exec -it ubuntu-kafka systemctl start kafka-connect.service
```
- status service
```bash
docker exec -it ubuntu-kafka systemctl status zookeeper.service
```
```bash
docker exec -it ubuntu-kafka systemctl status kafka.service
```
```bash
docker exec -it ubuntu-kafka systemctl status schema-registry.service
```
```bash
docker exec -it ubuntu-kafka systemctl status kafka-connect.service
```
## Create Topic
```bash
docker exec -it ubuntu-kafka /opt/confluent/bin/kafka-avro-console-producer --topic test \
    --broker-list localhost:9092 \
    --property value.schema='{"type":"record","name":"testrecord","fields":[{"name":"name","type":"string"}]}'
```
## Topics List
```bash
docker exec -it ubuntu-kafka /opt/confluent/bin/kafka-topics --list --bootstrap-server localhost:9092
```
## Consume Topic

```bash
docker exec -it ubuntu-kafka /opt/confluent/bin/kafka-avro-console-consumer --topic test-tabletest --bootstrap-server localhost:9092 --from-beginning
```

## Kafka Connect UI
```bash
docker run -d --rm -it -p 8100:8000 -e "CONNECT_URL=http://{KAFKA_CONNECT_IP}{KAFKA_CONNECT_PORT}" landoop/kafka-connect-ui
```

## Plugins Location
```
/opt/confluent/share/java
```
Or you can also access on mounted docker volume directory

Restart kafka connect after add or update plugin
```
docker exec -it ubuntu-kafka systemctl restart kafka-connect.service
```

docker registry
https://hub.docker.com/repository/docker/dhohirp/ubuntu-kafka/general

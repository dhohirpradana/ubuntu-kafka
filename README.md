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
systemctl enable zookeeper.service
systemctl enable kafka.service
systemctl enable schema-registry.service
systemctl enable kafka-connect.service
```
- start service
```bash
systemctl start zookeeper.service
systemctl start kafka.service
systemctl start schema-registry.service
systemctl start kafka-connect.service
```
- status service
```bash
systemctl status zookeeper.service
systemctl status kafka.service
systemctl status schema-registry.service
systemctl status kafka-connect.service
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

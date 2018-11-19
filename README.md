## Kafka Topics

### Create a topic
 `bin/kafka-topics.sh --create --zookeeper localhost:2181 --topic test-topic --partitions 2 --replication-factor 2`

### Increase the partition's count of topic
 `bin/kafka-topics.sh --zookeeper localhost:2181 --alter --topic test-topic --partitions 101`


### List existing topics
 `bin/kafka-topics.sh --zookeeper localhost:2181 --list`

### Describe a topic
  `bin/kafka-topics.sh --zookeeper localhost:2181 --describe --topic test-topic `
### Purge a topic
 `bin/kafka-topics.sh --zookeeper localhost:2181 --alter --topic test-topic --config retention.ms=1000`
 
... wait a minute ...

 `bin/kafka-topics.sh --zookeeper localhost:2181 --alter --topic test-topic --delete-config retention.ms`
 
### Delete a topic
 `bin/kafka-topics.sh --zookeeper localhost:2181 --delete --topic test-topic`

### Update the topic config
 `bin/kafka-configs.sh --zookeeper localhost:2181 --alter --entity-name test-topic --add-config 'retention.ms=1000' --entity-type topics`
 
### Get number of messages in a topic ???
 `bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic test-topic --time -1 --offsets 1 | awk -F  ":" '{sum += $3} END {print sum}'`
 
### Get the earliest offset still in a topic
`bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic test-topic --time -2`

### Get the latest offset still in a topic
`bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list localhost:9092 --topic test-topic --time -1`

### Produce messages with the console producer
`bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test-topic`

### Consume messages with the console consumer
`bin/kafka-console-consumer.sh --new-consumer --bootstrap-server localhost:9092 --topic test-topic --from-beginning`

## Get the consumer offsets for a topic
`bin/kafka-consumer-offset-checker.sh --zookeeper=localhost:2181 --topic=test-topic --group=my_consumer_group`

### Read from __consumer_offsets

Add the following property to `config/consumer.properties`:
`exclude.internal.topics=false`

`bin/kafka-console-consumer.sh --consumer.config config/consumer.properties --from-beginning --topic __consumer_offsets --zookeeper localhost:2181 --formatter "kafka.coordinator.GroupMetadataManager\$OffsetsMessageFormatter"`

## Kafka Consumer Groups

### List the consumer groups known to Kafka
`bin/kafka-consumer-groups.sh --zookeeper localhost:2181 --list`  (old api)

`bin/kafka-consumer-groups.sh --new-consumer --bootstrap-server localhost:9092 --list` (new api)

### View the details of a consumer group 
`bin/kafka-consumer-groups.sh --zookeeper localhost:2181 --describe --group <group name>`

## kafkacat

### Getting the last five message of a topic
`kafkacat -C -b localhost:9092 -t test-topic -p 0 -o -5 -e`

## Zookeeper

### Starting the Zookeeper Shell

`bin/zookeeper-shell.sh localhost:2181`

## Kafka Topics

### Create a topic
 `bin/kafka-topics.sh --create --zookeeper <zookeeperhost:port> --topic <topic_name> --partitions 2 --replication-factor 2`

### Increase the partition's count of topic
 `bin/kafka-topics.sh --zookeeper <zookeeperhost:port> --alter --topic <topic_name> --partitions 101`


### List existing topics
 `bin/kafka-topics.sh --zookeeper <zookeeperhost:port> --list`

### Describe a topic
  `bin/kafka-topics.sh --zookeeper <zookeeperhost:port> --describe --topic <topic_name>`
  
### Purge a topic
 `bin/kafka-topics.sh --zookeeper <zookeeperhost:port> --alter --topic <topic_name> --config retention.ms=1000`
 
... wait for 5 minutes ...

 `bin/kafka-topics.sh --zookeeper <zookeeperhost:port> --alter --topic <topic_name> --delete-config retention.ms`
 
### Delete a topic
 `bin/kafka-topics.sh --zookeeper <zookeeperhost:port> --delete --topic <topic_name>`

### Update the topic config
 `bin/kafka-configs.sh --zookeeper <zookeeperhost:port> --alter --entity-name <topic_name> --add-config 'retention.ms=1000' --entity-type topics`
 
### Get number of messages in a topic ???
 `bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list <kafkahost:port> --topic <topic_name> --time -1 --offsets 1 | awk -F  ":" '{sum += $3} END {print sum}'`
 
### Get the earliest offset still in a topic
`bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list <kafkahost:port> --topic <topic_name> --time -2`

### Get the latest offset still in a topic
`bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list <kafkahost:port> --topic <topic_name> --time -1`

### Produce messages with the console producer
`bin/kafka-console-producer.sh --broker-list <kafkahost:port> --topic <topic_name>`

### Consume messages with the console consumer
`bin/kafka-console-consumer.sh --new-consumer --bootstrap-server <kafkahost:port> --topic <topic_name> --from-beginning`

## Get the consumer offsets for a topic
`bin/kafka-consumer-offset-checker.sh --zookeeper=<zookeeperhost:port> --topic=<topic_name> --group=my_consumer_group`

### Read from __consumer_offsets

Add the following property to `config/consumer.properties`:
`exclude.internal.topics=false`

`bin/kafka-console-consumer.sh --consumer.config config/consumer.properties --from-beginning --topic __consumer_offsets --zookeeper <zookeeperhost:port> --formatter "kafka.coordinator.GroupMetadataManager\$OffsetsMessageFormatter"`

## Kafka Consumer Groups

### List the consumer groups known to Kafka
`bin/kafka-consumer-groups.sh --zookeeper <zookeeperhost:port> --list`  (old api)

`bin/kafka-consumer-groups.sh --new-consumer --bootstrap-server <kafkahost:port> --list` (new api)

### Describe the consumer group (TOPIC, PARTITION, CURRENT-OFFSET, LOG-END-OFFSET, LAG, CONSUMER-ID, HOST and CLIENT-ID)
`./bin/kafka-consumer-groups.sh --bootstrap-server <kafkahost:port> --describe --group <group_name>`


### View the details of a consumer group 
`bin/kafka-consumer-groups.sh --zookeeper <zookeeperhost:port> --describe --group <group_name>`

### Reset the consumer offset for a topic (preview)
`bin/kafka-consumer-groups.sh --bootstrap-server <kafkahost:port> --group <group_id> --topic <topic_name> --reset-offsets --to-earliest`

This will print the expected result of the reset, but not actually run it.

### Reset the consumer offset for a topic (execute)
`bin/kafka-consumer-groups.sh --bootstrap-server <kafkahost:port> --group <group_id> --topic <topic_name> --reset-offsets --to-earliest --execute`

This will execute the reset and reset the consumer group offset for the specified topic back to 0.

## kafkacat

### Getting the last five message of a topic
`kafkacat -C -b <kafkahost:port> -t <topic_name> -p 0 -o -5 -e`

## Zookeeper

### Starting the Zookeeper Shell

`bin/zookeeper-shell.sh <zookeeperhost:port>`

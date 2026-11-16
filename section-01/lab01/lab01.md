# create input topic with 3 partitions
bin/kafka-topics.sh --bootstrap-server $BOOTSTRAP_SERVER --command-config $KAFKA_CONFIG --create --replication-factor 3 --partitions 3 --topic streams-plaintext-input


# create output topic
bin/kafka-topics.sh --bootstrap-server $BOOTSTRAP_SERVER --command-config $KAFKA_CONFIG  --create --replication-factor 3 --partitions 3 --topic streams-plaintext-output

kafka-console-producer.sh \
  --bootstrap-server "$BOOTSTRAP_SERVER" \
  --producer.config client.properties \
  --topic streams-plaintext-input

>hello kafka
>hello streams
>kafka streams kafka

# launch a Kafka consumer

# start a consumer on the output topic
bin/kafka-console-consumer.sh --bootstrap-server $BOOTSTRAP_SERVER \
    --consumer.config $KAFKA_CONFIG \
    --topic streams-plaintext-output\
    --from-beginning \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer


kafka-console-consumer.sh --bootstrap-server $BOOTSTRAP_SERVER \
    --consumer.config $KAFKA_CONFIG \
    --topic streams-wordcount-output\
    --from-beginning \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer

# launch the streams application
export CLASSPATH="/home/ubuntu/confluent-client/kafka-streams-cources.jar"
kafka-run-class.sh   com.demo.streams.examples.WordCountDemo   --bootstrap-server "$BOOTSTRAP_SERVER"   --kafka-config client.properties

kafka-run-class.sh   com.demo.streams.examples.WordCountLambdaExample   --bootstrap-server "$BOOTSTRAP_SERVER"   --kafka-config client.properties


Need to install kafka.

Then start the zookeeper server 
> bin/zookeeper-server-start.sh config/zookeeper.properties

Then start the kafka server:
> bin/kafka-server-start.sh config/server.properties

Prepare input topic and start kafka producer:
> bin/kafka-topics.sh --create \
    --zookeeper localhost:2181 \
    --replication-factor 1 \
    --partitions 1 \
    --topic streams-plaintext-input
    
> bin/kafka-topics.sh --create \
    --zookeeper localhost:2181 \
    --replication-factor 1 \
    --partitions 1 \
    --topic streams-pipe-output
    
Run the application:
> mvn exec:java -Dexec.mainClass=com.fgaudin.kafkastreamexperiment.Pipe

Inspect the output
> bin/kafka-console-producer.sh --broker-list localhost:9092 --topic streams-plaintext-input
> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
    --topic streams-pipe-output \
    --from-beginning \
    --formatter kafka.tools.DefaultMessageFormatter \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
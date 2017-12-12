# datastreaming-presentation
Datastreaming presentation

Presentation Slides: https://github.com/smarcu/datastreaming-presentation/blob/master/data-streaming-presentation.pdf

## Generate a sample kafka streaming project

```
mvn archetype:generate \
    -DarchetypeGroupId=org.apache.kafka \
    -DarchetypeArtifactId=streams-quickstart-java \
    -DarchetypeVersion=1.0.0 \
    -DgroupId=com.streams.examples \
    -DartifactId=kafka-streaming \
    -Dversion=0.1 \
    -Dpackage=com.streams.examples.kafka.streaming
```

## Install Confluent Open Source tools

 * https://www.confluent.io/download/
 * add confluent bin folder to path
 
 
## Example 1 - Pipe

In this example, we implement a simple LineSplit program using the high-level Streams DSL
that reads from a source topic "streams-plaintext-input", where the values of messages represent lines of text,
and writes the messages as-is into a sink topic "streams-pipe-output".

<img src="kafka-streaming-example1.png" alt="Example 1 - Pipe" style="width: 600px;"/>

### Setup Kafka

Check status of kafka 

```
confluent status
```

Start kafka

```
confluent start
```
 
Create kafka topics

```
kafka-topics --create \
          --zookeeper localhost:2181 \
          --replication-factor 1 \
          --partitions 1 \
          --topic streams-plaintext-input
```
 
```
kafka-topics --create \
          --zookeeper localhost:2181 \
          --replication-factor 1 \
          --partitions 1 \
          --topic streams-pipe-output
```
 
Verify topics
 
```
kafka-topics --list --zookeeper localhost:2181
```

Start kafka stream processor
* run the `class com.streams.examples.kafka.streaming.Pipe`

Start a simple console consumer on topic streams-pipe-output

```
kafka-console-consumer --bootstrap-server localhost:9092 \
          --topic streams-pipe-output \
          --from-beginning \
          --formatter kafka.tools.DefaultMessageFormatter \
          --property print.key=true \
          --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
          --property value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

Start a simple console producer

```
kafka-console-producer --broker-list localhost:9092 --topic streams-plaintext-input
```

## Example 2 - LineSplit

In this example, we implement a simple LineSplit program using the high-level Streams DSL
that reads from a source topic "streams-plaintext-input", where the values of messages represent lines of text;
the code split each text line in string into words and then write back into a sink topic "streams-linesplit-output" where
each record represents a single word.


### Setup Kafka

Create `streams-linesplit-output` topic

```
kafka-topics --create \
          --zookeeper localhost:2181 \
          --replication-factor 1 \
          --partitions 1 \
          --topic streams-linesplit-output
```

Start a simple console consumer on topic streams-linesplit-output

```
kafka-console-consumer --bootstrap-server localhost:9092 \
          --topic streams-linesplit-output \
          --from-beginning \
          --formatter kafka.tools.DefaultMessageFormatter \
          --property print.key=true \
          --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
          --property value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

## Example 3 - Word Count

In this example, we implement a simple WordCount program using the high-level Streams DSL
that reads from a source topic "streams-plaintext-input", where the values of messages represent lines of text,
split each text line into words and then compute the word occurence histogram, write the continuous updated histogram
into a topic "streams-wordcount-output" where each record is an updated count of a single word.


### Setup Kafka

Create `streams-wordcount-output` topic

```
kafka-topics --create \
          --zookeeper localhost:2181 \
          --replication-factor 1 \
          --partitions 1 \
          --topic streams-wordcount-output
```

Start a simple console consumer on topic streams-linesplit-output

```
kafka-console-consumer --bootstrap-server localhost:9092 \
          --topic streams-wordcount-output \
          --from-beginning \
          --formatter kafka.tools.DefaultMessageFormatter \
          --property print.key=true \
          --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
          --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```

## Dockerized 3 Broker Cluster

* Install Docker https://www.docker.com/get-docker
* Fetch the github project: https://github.com/wurstmeister/kafka-docker
* Follow the instructions on kafka-docker (update docker-compose.yml)



# datastreaming-presentation
Datastreaming presentation


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
 
## Setup Kafka

 
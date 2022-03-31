# What its like to eventify all requests and responces (MMOify the Web)

## Event Driven Architecture

## Kafka Components

### Broker API (9092)

A whitebox with tons of abstractions, allowing producers and consumers pipeline to exist and function in a very fast latency, by using binary protocol over TCP AKA machine read protocol.

The broker contains ***topics*** as in a bunch of containers that in its own, contain messages that producers produce, for the consumers to consume, in a form that resembles a queue and behaves like it but is not quite a queue.

### Producers

A start-point machine that sends "requests" in form of messages to a specific topic.

### Consumers

An end-point machine that reacts to "requests" and does operations accordingly.

## Big Brokers and small partitions

Big topics, producers and consumers require a split in responsibility with Kafka partitions ( just as with databases sharding ) within the same topic.


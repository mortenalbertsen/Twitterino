# Twitterino

This project is an exercise from Udemy Course "[Learn Apache Kafka for Beginners v2](https://www.udemy.com/course/apache-kafka/)"



This project contains

- A `TwitterClient` that receives a stream of tweets from subscribed Twitter-channels (such as "cnn") and twitter-handles, such as "#omikron", and hands these tweets off to...
- ...a `KafkaPublisher`, that pushes these tweets onto a Kafka-topic, "tweets"
- A `TweetConsumer`, that consumes tweets off of topic `tweets` and pushes these into an ElasticSearch index named `twitterdata`
- docker-compose file for "up'ing" a Kibana + Elasticsearch stack
- docker-compose file for "up'ing" a Kafka stack



## Prerequisities

In order to run this stack you'll need

- A Twitter-developer account (free), specifically
  - a bearer-token is required in order to connect to Twitter's API
- `docker` installed
- `kafka` command-line utilities installed, see [this quick-start guide](https://kafka.apache.org/quickstart) to get set up.
- Java installed
- Gradle installed



### Additional setup

In order to stream tweets you'll also need to setup some *filtered stream rules*. Check out Twitter's developer documentation. Currently the ids of my filtered stream rules are hardcoded into `KafkaPublisher` and you'll need to adjust these accordingly. 



## Run ElasticSearch and Kibana stack

From `docker-compose` folder, run

```
docker-compose -f elastic.yaml up -d
```

Now run ...

```
docker ps
```

...to check that two containers are now running

You should also be able to navigate to http://localhost:5601/ to get the Kibana frontend. 



## Run Kafka (and zookeeper)

From `docker-compose` folder, run

```
docker-compose -f kafka.yaml up -d
```

Now run ...

```
docker ps
```

...to check that a `zookeeper` and three `kafka` containers are now running



### One-time setup of `Tweets` kafka-topic (commandline)

This assumes you have the `kafka-topics` command-line utility installed. Check [this quick-start guide](https://kafka.apache.org/quickstart) to get set up.

Assuming `kafka` and `zookeeper` are already running, run

```
kafka-topics.sh --bootstrap-server 127.0.0.1:9092 --create --topic tweets --partitions 3 --replication-factor 3 --config min.insync.replicas 2
```





### One-time setup of `Tweets` kafka-topic (GUI)

Look into [Conduktor](https://www.conduktor.io/) (requires Java 11), and create a topic named `tweets` with a replication-factor of 1 and with a single partition. 



## Other maybe-useful commands



### Delete topic

```
kafka-topics.sh --bootstrap-server 127.0.0.1:9092 --delete --topic tweets
```



### Listing topics

``` 
kafka-topics.sh --bootstrap-server 127.0.0.1:9092 --list
```



### List messages on a topic

```
kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic tweets
```

Note that this command 

- does not return before you stop it (`Ctrl`+`c`)
- does not show/print anything until messages have been posted to the topic



### Describe a topic


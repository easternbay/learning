# Introduction #
Message queues provide a capablity of asynchronous communication, which means that the sender and receiver of the message do not need to interact with the queue at the same time. One client can simply put the message onto the queue, and the others can read the message at other time. This is important when your applications grow bigger.With the message queue technologies, we can simplify decoupled applications and improving the performance, reliability and scalability of it.

Many implementations of message queues function within an operating system or an application. In this case, their capabilities may be limited. There are also implementations allow the communication between different computer systems or applications. These message queue implementations typically provide the ability to ensure the messages do not get lost. There are many open choices of messaging middleware systems, including Apache ActiveMQ, RabbitMQ, Apache Kafka and so on. There are also cloud-based message queuing service, such as Amazon Simple Queue Serivce(SQS), StormMQ. In normally Java application, the Spring Framework also provide an annotation called @Async to provide the support of asynchronous communication. In the next part of the article, we will discuss the distinctions of ActiveMQ, RabbitMQ, Apache kafka, Amazon SQS and the @Async annotation.

# ActiveMQ #
Main Features of ActiveMQ
- Cross-language compilance and compatibility: Allowing cross-language compatibility allows communication between very diverse applications, and allows seamsless interation.
- Ajax and REST support
- Compatible with various transport protocols including TCP, UDP and SSL
- Support for persistence
- High-performance computing and high availability: This is mainly through clustering.

In order to use ActiveMQ, you need to download and install it on your computer or somewhere else like a server. It provides a default administrative interface on http://127.0.0.1:8161/admin/ with username 'admin' and password 'admin'. Once the ActiveMQ broker runs, you can configure the broker by specifying an Xml Configuration file  as a parameter to the activemq command or by Broker Configuration URI on command line.

Other Features of ActiveMQ
1. Supports advisory messages which allows you to watch the system using regular JMS messages. Advisory messages can be thought as some kind of administrative channel where you receive information regarding what is happening on your JMS provider along with what's happening with producers, consumers and destinations.
2. ActiveMQ supports Cluster. The most common menntal model of clustering in a JMS context is that there is a collection of JMS brokers and a JMS client will connect to one of them; then if the JMS broker goes down, it will auto-reconnect to another broker. ActiveMQ also supports a Networks of Brokers which provides store and forward to move messages from brokers with producers to brokers with consumers which allows us to support distributed queues and topics across a network of brokers.
3. ActiveMQ supports multiple program language such as Java, Python, C++ and so on.
4. ActiveMQ is a message broker written in Java with JMS, REST and WebSocket interfaces.
5. ActiveMQ provides bridging functionality to other JMS providers that implement the JMS 1.0.2 and above specification.
6. ActiveMQ comes with a feature called MasterSlave, that is, if you have a hardware failure of the master's machine, file system or data centre, you get immediate failover to the slave with no message loss.
7. Message Groups provide:
 - Ordering of the processing of related messages across a single queue.
 - Load banlancing of the processing of messages across multiple consumers.
 - High availability/auto-failover to other consumers if a JVM goes down. 

8. From 1.1 onwards of ActiveMQ supports networks of brokers which allows us to support distributed queues and topics across a network of brokers.
9. Virtual Destinations allow us to create logical destinations that clients can use to produce and consume from but which map onto one or more physical destinations.


# RabbitMQ #
RabbitMQ is written in the Erlang programming language and it's client libraries to interface with the broker are availablefor all major programming languages. It implements the Advanced Message Queuing Protocol(AMQP) and has benn extended with a plug-in architecture to support Streaming Text Oriented Messaging Protocol(STOMP), Message Queuing Telemetry Trnsport(MQTT), and other protocols.

#### AMQP ####
##### introduction #####
AMQP 0-9-1 is a messaging protocol that enables conforming client applications to communicate with conforming middleware brokers.
##### AMQP Model ####
Messages are published to exchanges. Exchanges then distribute message copies to queues using rules called bindings. Then AMQP brokers either deliver messages to consumers subscribed to queues, or consumers fetch/pull messages from queues on demand.

##### Exchanges and Exchange Types #####
AMQP 0-9-1 is a programmable protocol in that AMQP 0-9-1 enties and routing schemes are primarily defined by applications themselves. Exchanges are AMQP entities where messages are sent. Exchanges take a message and route it into zero or more queues.AMQP 0-9-1 brokers provide four exchange types:
- Direct exchange
- Fanout exchange
- Topic exchange
- Headers exchange

Queues in the AMQP 0-9-1 are similar to queues of other messaging and queueing systems: they store messages that are  consumed by applications.
##### other features ####
AMQP 0-9-1 has a built-in feature called message acknowledgements (sometimes referred to as acks) that consumers use to confirm message delivery and/or processing. 

When multiple consumers share a queue, it is useful to specify how many messages each consumer can be sent at once before sending next acknowledgement.

To make it possible for a single broker to host multiple isolated "environments", AMQP includes the concept of virtual hosts.

All RabbitMQ brokers start out as running on a single node. These nodes can be joined into clusters, and subsequently turned back into individual brokers again. 

RabbitMQ is officially supported on a number of operating systems and several languages.The RabbitMQ community has created numerous clients, adaptors and tools.

RabbitMQ has pluggable support for various SASL authentication mechanisms.


# Apache Kafka #
#### Introduction ####
Apache Kafka is an open-source stream-processing software platform developed by the Apache Software Foundation, written in Scala and Java.

Kafka has four core APIs:
- **The Producer API** allows an application to publish a stream of records to one or more Kafka topics.
- **The Consumer API** allows an application to subscribe to one or more topics and process the stream of records produced to them.
- **The Streams API** allows an application to act as a stream processor, consuming an input stream from one or more topics and producing an output stream to one or more output topics, effectively transforming the input streams to output streams.
- **The Connector API** allows building and running reusable producers or consumers that connect Kafka topics to existing applications or data systems. For example, a connector to a relational database might capture every change to a table.

A few concepts:
- Kafka is run as a cluster on one or more servers that can span multiple datacenters.
- The Kafka cluster stores streams of records in categories called topics.
- Each record consists of a key, a value, and a timestamp.



Kafka supports two types of topics: regular and compacted:
- Regular topics can be configured with a retention time or space bound. If there are records that are older than the specified retention time or the space bound is exceeded for a partition, Kafka is allowed to delete old data to free storage space. By default, topics are configured with a retention time of 7 days but it's also possible to store data indefinitely.
- As to compacted topics, Kafka treats later messages as updates to older message with the same key and guarantees to never delete the latest message per key. Users can delete messages entirely by writing a so-called tombstone message with null-value for a specific key.

#### Features ####
- For each topic, the Kafka cluster maintains a partitioned log. Each partition is an ordered, immutable sequence of records that is continually appended to—a structured commit log.The records in the partitions are each assigned a sequential id number called the offset that uniquely identifies each record within the partition.
- The partitions of the log are distributed over the servers in the Kafka cluster with each server handling data and requests for a share of the partitions. Each partition is replicated across a configurable number of servers for fault tolerance.
- Messages sent by a producer to a particular topic partition will be appended in the order they are sent. A consumer instance sees records in the order they are stored in the log.
- As Traditional messaging has two model: queuing and publish-subscribe. But they have liminations. Queues aren't multi-subscriber—once one process reads the data it's gone.  Publish-subscribe has no way of scaling processing since every message goes to every subscriber. It is different here. As with a queue the consumer group allows you to divide up pprocessing over a collection of processes. As with publish-scribe, Kafka allows you to broadcast messages to multiple consumer groups.
-  Kafka allows producers to wait on acknowledgement so that a write isn't considered complete until it is fully replicated and guaranteed to persist even if the server written to fails.
- Data written to Kafka is written to disk and replicated for fault-tolerance.
- The streams API built on the producer and consumer API enables you to process real-time streams.
- By combining storage and subscriptions, streaming applications can treat both past and future data the same way.
- Kafka authenticcate connections to brokers from cliens , other brokers and tools using either SSL or SASL. Support Authentication of connections from brokers to ZooKeeper which Kafka bases on to implements cluster. Encrypt the data transfered between brokerss and clients, between brokers, or between brokers and tools using SSL. Authorization of read / write operations by clients. All of these security policies are optional.


# @Async #
#### Introduction ####
You can enable asychronous processing with Java configuration - by simply adding the @EnableAsync to a configuration class. Asynchronous processing can also be enabbled using XML configuration by using the task namespace:
```
<task:executor id="myexecutor" pool-size="5"  />
<task:annotation-driven executor="myexecutor"/>
```

@Async has two limitations:
- it must be applied to public methods only
- self-invocation - calling the async method from within the same class - won't work.

The reasons are simple – the method needs to be public so that it can be proxied. And self-invocation doesn’t work because it bypasses the proxy and calls the underlying method directly.

#### The return type ####
@Async can be applied to a method with return type wrapped by the actual return in the Future. It can also be aplied to a method with void return type.
```
@Async
public Future<String> asyncMethodWithReturnType() {
    System.out.println("Execute method asynchronously - "
      + Thread.currentThread().getName());
    try {
        Thread.sleep(5000);
        return new AsyncResult<String>("hello world !!!!");
    } catch (InterruptedException e) {
        //
    }
 
    return null;
}
```

```
@Async
public void asyncMethodWithVoidReturnType() {
    System.out.println("Execute method asynchronously. "
      + Thread.currentThread().getName());
}
```
#### The Executor ####
By default, Spring uses a SimpleAsyncTaskExecutor to actually run these methods asynchronously. The defaults can be overridden at two levels – at the application level or at the individual method level.
- At the individual level, the required executor needs to be declared in a configuration class, and then the executor name should be provided as an attribute in @Async.
- At the application level, the configuration class should implement the AsyncConfigurer interface - which mean that it has implement the getAsyncExecutor() method. It’s here that we will return the executor for the entire application – this now becomes the default executor to run methods annotated with @Async

#### Exception Handling ####
When a method return type is a Future, exception handling is easy – Future.get() method will throw the exception.  

But, if the return type is void, exceptions will not be propagated to the calling thread. Hence we need to create a custom async exception handler by implementing AsyncUncaughtExceptionHandler interface. The handleUncaughtException() method is invoked when there are any uncaught asynchronous exceptions



# Amazon SQS #
#### Introduction ####
Amazon Simple Queue Service(SQS) is a cloud-based, fully managed message queuing service that enables you to decoupple and scale microservices, distributed systems, and serverless applicationns. SQS eliminates the complexity and overhead associated with managing and operating message oriented middleware.

SQS offers to types of message queues:
- Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery.
- SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.

#### Features ####
- AWS manages all ongoing operations and underlying infrastructure nedded to provide a highly available and scalable message queuing service.
- Use Amazon SQS to transmit any volume of data, at any level of throughput, without losing messages or requiring other services to be available.
- You can use Amazon SQS to exchange sensitivve data between applications using server-side encryption(SSE) to encrypt each messaage body.
- Amazon SQS leverages the AWS cloud to dynamicaly scale based on demand. SQS scales elastically with your application soo you don't have to worry about capacity planing and pre-provision.
- The Standatd Queues support a nearly unlimited number of transactions per second(TPS) per API action. By default, FIFO queues support up to 300 messages per second. When you batch 10 messages pesr operation(maximum), FIFO queues can support up to 3000 messages per second.
- Message payloads can contain up to 256KB of text in any format. Each 64KB 'chunk' of payload is billed as 1 request.
- Send, receive, or delete messages in batches of up to 10 messages or 256KB.
- Retain messages in queues for up to 14 days.
- When a message is received, it becomes "locked" while being processed. If the message processing fails, the lock will expire and the message will be availale again.
- Handle messages that have not been successfully processed by a consumer with Dead Letter Queues. When the maximum receive count is exceeded for a message it will be moved to the DLQ associated with the original queue.
- Securely share Amazon SQS queues anonymously or with specific AWS accounts. Queue sharing can also be restricted by IP address and time-of-day.

---
https://en.wikipedia.org/wiki/Message_queue  
https://opensourceforu.com/2011/04/understanding-middleware-with-apache-activemq/  
http://activemq.apache.org/getting-started.html  
https://www.rabbitmq.com/  
https://en.wikipedia.org/wiki/Apache_Kafka  
https://kafka.apache.org/intro  
https://aws.amazon.com/sqs/  
http://www.baeldung.com/spring-async  

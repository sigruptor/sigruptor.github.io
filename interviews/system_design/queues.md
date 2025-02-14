# Queueing Systems Comparison: Kafka, SQS, Google Pub/Sub, RabbitMQ

## Key Comparison Table

| Feature            | **Apache Kafka** | **Amazon SQS** | **Google Pub/Sub** | **RabbitMQ** |
|--------------------|----------------|----------------|----------------|----------------|
| **Message Model** | Log-based event streaming (append-only log). Consumers track their own offsets. | Queue-based message passing (FIFO or standard queue). | Pub/Sub event distribution (similar to Kafka but managed). | Queue-based (push or pull). |
| **Multiple Subscribers** | ✅ Yes (multiple consumer groups, each getting a full copy of the topic). | ⚠️ No native fan-out (use SNS for multiple consumers). | ✅ Yes (Pub/Sub model allows multiple subscribers). | ✅ Yes (competing consumers model, but per queue). |
| **Cost** | Free (self-hosted), but high infra costs (storage, brokers, Zookeeper). | Pay-per-request (based on API calls & message size). | Pay-per-message (subscription and throughput based). | Free (self-hosted), infra costs for clusters. Managed RabbitMQ incurs cost. |
| **Complexity** | **High** (requires managing brokers, partitions, offsets, scaling). | **Low** (fully managed, no infra to maintain). | **Medium** (managed service but requires topic/subscription management). | **Medium** (easier than Kafka but complex in HA setups). |
| **At-Least / At-Most Once** | **At-least-once** (default), **Exactly-once** (via transactions). | **At-least-once** (Standard Queue), **Exactly-once** (FIFO Queue). | **At-least-once** (default), **Exactly-once** (enabled via deduplication). | **At-least-once** (by default), **At-most-once** (if auto-ack enabled). |
| **Fault Tolerance** | ✅ High (replicated partitions across brokers). | ✅ High (AWS handles durability and replication). | ✅ High (Google replicates messages across zones). | ✅ High (via clustering and mirrored queues). |
| **Partitioning** | ✅ Yes (sharding via topic partitions). | ❌ No (FIFO queue supports deduplication but no partitioning). | ✅ Yes (messages routed across multiple subscriptions). | ❌ No (queues do not support partitioning). |
| **Ordering** | ✅ Yes (guaranteed within a partition). | ✅ FIFO queues support strict ordering; standard queues do not. | ⚠️ Not strictly ordered (ordering per subscription is not guaranteed). | ✅ Yes (queue-level strict ordering). |
| **Message Retention** | **Configurable** (days to forever). Consumers track offsets. | **4 days** (14-day max with Extended Retention). | **7 days** (configurable). | **Until consumed** (or TTL expires). |
| **Delivery Guarantee** | **Pull-based** (consumer-driven, replayable). | **Push-based** (messages auto-deleted after successful delivery). | **Push or pull** (subscriptions define behavior). | **Push or pull** (acknowledgment determines delivery). |
| **Scaling** | ✅ High (horizontal scaling via partitions). | ✅ High (AWS-managed, virtually infinite scaling). | ✅ High (Google-managed, automatically scales). | ⚠️ Limited (scales vertically; clustering helps but not as scalable as Kafka). |
| **Use Cases** | Event-driven systems, analytics, logs, real-time streaming, pub/sub messaging. | Background jobs, decoupling microservices, queueing tasks. | Large-scale pub/sub messaging, cloud-native apps, event distribution. | Request-response, IoT, transactional messaging, message routing. |


## Comparison of Queueing Technologies in detail
This document provides a detailed comparison of various queueing technologies, focusing on key features such as partitioning, delivery semantics, scalability, cost, and complexity.

| Feature                 | Apache Kafka                                                                                         | RabbitMQ                                                                                             | Amazon SQS                                                                                         | Google Pub/Sub                                                                                     |
|-------------------------|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **Partitioning**        | Supports partitioning, allowing data to be split across multiple partitions for parallel processing. | Does not natively support partitioning; uses exchanges and queues for message routing.              | Does not support partitioning; messages are distributed across queues.                             | Supports partitioning through topics and subscriptions, enabling parallel processing.             |
| **Delivery Semantics**  | Offers at-least-once and exactly-once delivery semantics, ensuring reliable message delivery.        | Provides at-most-once and at-least-once delivery semantics, with support for message acknowledgments. | Supports at-least-once delivery; FIFO queues ensure ordered processing.                            | Offers at-least-once delivery; messages can be ordered within a subscription.                     |
| **Scalability**         | Highly scalable; can handle millions of events per second by adding more brokers.                   | Scales horizontally by adding more nodes; may require manual intervention for optimal performance.   | Automatically scales to handle increased message volumes without user intervention.                | Automatically scales with demand, managing resource allocation dynamically.                        |
| **Cost**                | Open-source; infrastructure and operational costs depend on deployment and maintenance.              | Open-source; costs are associated with the underlying hardware and maintenance efforts.              | Pay-as-you-go model; costs are based on the number of requests and data transfer.                 | Pay-as-you-go model; pricing is based on data volume and operations performed.                     |
| **Complexity**          | Requires setup and configuration; understanding of partitions, brokers, and offset management is necessary. | Simpler setup; suitable for straightforward messaging tasks but may become complex with scaling.   | Fully managed service; minimal setup required, making it user-friendly.                           | Fully managed service; integrates well with other Google Cloud services, offering ease of use.    |

This comparison aims to assist in selecting the appropriate queueing technology based on specific requirements and constraints.


### Summary: When to Use What?

| **Scenario** | **Best Choice** |
|-------------|---------------|
| **Real-time event streaming (logs, analytics, clickstream, telemetry, financial data processing, Kafka Streams processing, high-throughput messaging).** | **Kafka** |
| **Simple, fully managed queueing for microservices (task queues, background processing, job scheduling, AWS-based applications).** | **SQS** |
| **Scalable, cloud-native messaging with automatic scaling, IoT telemetry, notifications, large-scale event distribution.** | **Google Pub/Sub** |
| **Lightweight message broker for traditional messaging patterns (RPC, job queues, real-time communication).** | **RabbitMQ** |


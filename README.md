# MassTransit overview <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What does MassTransit?](#1-what-does-masstransit)
- [2. Messages](#2-messages)
  - [2.1. Message Names](#21-message-names)
- [3. Consumers](#3-consumers)
- [4. Producers](#4-producers)

# 1. What does MassTransit?

Is a **message bus**, free, open-source distributed application framework for .NET. MassTransit makes it easy to create applications and services that leverage message-based, loosely-coupled asynchronous communication for higher availability, reliability, and scalability.

- MassTransit is a lightweight service bus for building distributed .NET applications. The main goal is to provide a consistent, .NET friendly abstraction over the message transport. To meet this goal, MassTransit brings a lot of the application-specific logic closer to the developer in an easy to configure and understand manner.

- Transports

  - MassTransit support multiple transports, including:
    - RabbitMQ
    - Azure Service Bus
    - ActiveMQ
    - Amazon SQS
    - In Memory

- Befenefits
  - Concurrency
  - Connection management
  - Exception, retries, and poison messages
  - Serialization
  - Consumer lifecycle management
  - Routing
  - Easy Unit Testing
  - Scheduling
  - Monitoring

# 2. Messages

- A **message** contract is defined code first by creating a .NET type. A message can be defined using a class or an interface, resulting in a strongly-typed contract. Messages should be limited to read-only properties and not include methods or behavior.
  - **Important**
    - MassTransit uses the full type name, including the namespace, for message contracts. When creating the same message type in two separate projects, the namespaces must match or the message will not be consumed.

## 2.1. Message Names

- There are two main message types, events and commands. When choosing a name for a message, the type of message should dictate the tense of the message
  - Commands
    - A command tells a service to do something. Commands are sent (using Send) to an endpoint, as it is expected that a single service instance performs the command action. A command should never be published.
    - Commands should be expressed in a verb-noun sequence, following the tell style.
    - Example Commands:
      - UpdateCustomerAddress
      - UpgradeCustomerAccount
      - SubmitOrder
  - Events
    - An event signifies that something has happened. Events are published (using Publish) via either IBus (standalone) or ConsumeContext (within a message consumer). An event should not be sent directly to an endpoint.
    - Events should be expressed in a noun-verb (past tense) sequence, indicating that something happened
    - Example Events:
      - CustomerAddressUpdated
      - CustomerAccountUpgraded
      - OrderSubmitted, OrderAccepted, OrderRejected, OrderShipped

# 3. Consumers

- Consumer is a widely used noun for something that consumes something. In MassTransit, a consumer consumes one or more message types when configured on or connected to a receive endpoint. MassTransit includes many consumer types, including:
  - Consumers
  - Sagas
  - Saga state machines
  - Routing slip activities
  - Handlers
  - Job consumers.
- A consumer, which is the most common consumer type, is a class that consumes one or more messages types. For each message type, the IConsumer<T> interface is implemented where T is the consumed message type. The interface has one method, Consume, as shown below.

  ```
  public interface IConsumer<in TMessage> :IConsumer where TMessage : class
  {
      Task Consume(ConsumeContext<TMessage> context);
  }
  ```

  # 4. Producers

- An application or service can produce messages using two different methods. A message can be sent or a message can be published. The behavior of each method is very different, but it's easy to understand by looking at the type of messages involved with each particular method.
- When a message is sent, it is delivered to a specific endpoint using a DestinationAddress. When a message is published, it is not sent to a specific endpoint, but is instead broadcasted to any consumers which have subscribed to the message type. For these two separate behavior, we describe messages sent as commands, and messages published as events.

- RabbitMQ
  - It is a **message broker** that helps us make async communication between microservice or two services. In other words, it is a message-queued that enables messaging for common platforms.

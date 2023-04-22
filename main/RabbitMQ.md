Tags: #backend 
Created: 2023-04-22 20:04
References: https://www.rabbitmq.com/getstarted.html

# RabbitMQ
RabbitMQ is a message broker. You can run it in a [[Docker]] container with the following command:

```sh
sudo docker run -it --rm \
	--name rabbitmq \
	-p 5672:5672 \
	-p 15672:15672 \
	-e RABBITMQ_DEFAULT_USER=user \
	-e RABBITMQ_DEFAULT_PASS=password \
	rabbitmq:3.11-management
```

## Concepts
**Producing** = Sending. A programs that sends messages is a *producer*.
**Queue** = The place where RabbitMQ stores messages. Many *producers* may send messages to a single queue and many *consumers* may try to receive messages from one.
**Consuming** = Receiving. A *consumer* is a program that mostly waits to receive messages.
**Exchange** = The place where *producers* send messages.

## Functionality
Things you can do with RabbitMQ.

### Connection
First thing that has to be done is to create a **connection** and get that *connection's* **channel**.
Before you finish your program, you need to **close** the *connection* that you created.

### Channel
Using the *channel* you can **declare** a **queue*** or even multiple *queues*, 
*Declaring* a queue can be done multiple times and is recommended to be done in both *producer* and *consumer*.
Using the *channel* you can **publish** messages.
Using the *channel* you can **consume** messages.
A *channel* can be configured to have a **prefetch count**, which is used for *fair dispatch*: "Don't give more that *{prefetch count}* messages to a consumer at a time (until it sent the *achnowledge*)".
Using the *channel* you can create a **binding**, which is a relationship between *exchanges* and *queues*.

### Queue
All *queues* have a **name**.
A *queue* may be **durable**, which means that messages will be resist even if the node that runs RabbitMQ fails.
It is possible to let RabbitMQ generate a *queue's name*.
A *queue* may be **exclusive**, which means that it will be deleted when the consumer's connection is closed.

### Message
A *message*'s properties contain the **delivery mode**, which can be set to `PERSISTENT_DELIVERY_MODE`, which means that the specific *message* will resist a node failure.
*Messages* can have a **route (routing key)** to which they are sent.

### Consumer
*Consumers* need to send and **acknowledge** message when they are done processing a message.

### Exchange
*Exchanges* receive messages and (based on their **type**) send them to none/one/multiple *queues*.
*Exchanges* have a **name**.
*Exchange* types can be: `direct`, `topic`, `headers` and `fanout`.
*Exchange `fanout`* broadcasts all messages to *queues* it knows.
*Exchange `direct`* sends a message to the *queues* whose *binding key* exactly matches the *routing key* of the message.
*Exchange `topic`* sends messages to queues that have a *binding key* that represents a subscription that includes the *routing key of the message*.

### Binding
A *binding* can have a **routing (binding) key**. This key's meaning depends on the *exchange* type.
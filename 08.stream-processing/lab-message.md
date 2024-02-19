Kafka Messaging Lab Instructions

## Objective

Your task is to produce a message to a Kafka topic designated by the instructor. This will test your ability to interact with Kafka in a real-world scenario.

## Prerequisites

- SSH access to the `edge-1` node.
- A valid Kerberos ticket for authentication.

## Environment Setup

Remember to set up your environment with the Kafka brokers and ZooKeeper quorum details as provided in the course materials.

https://www.adaltas.cloud/en/docs/big-data/kafka-basics/

## Sending a Message

1. Utilize the Kafka console producer to produce a message to the topic named `student-messages-to-instructor`. Refer to course materials for the command syntax.
  
2. Construct a JSON message with the following structure, substituting placeholders with your actual information:
  
  - Your student ID
  - Your name
  - The current timestamp
  - A short message
  - Optional feedback about the course
3. Send your message to the Kafka topic. Ensure your message adheres to the JSON format for ease of processing.
  

## Completion

After sending your message, inform the instructor. Your participation in this lab exercise will be confirmed upon successful message delivery.

### Note

Pay close attention to the details provided in the course materials for commands and JSON message formatting. This exercise is designed to challenge your understanding and ability to apply what you've learned.

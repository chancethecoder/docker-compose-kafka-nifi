# docker-compose-kafka-nifi

Filebeat, Kafka, Nifi on docker-compose example (Tested on Mac)

## Requirements & used

- docker & docker-compose (version: 3+)
- [flog](https://github.com/mingrammer/flog)
- [kafkacat](https://github.com/edenhill/kafkacat)

## How to Run

1. Start processes

    ```bash
    docker-compose up -d
    ```

2. Generate data

    ```bash
    # set your own sleep interval
    while true; do docker run -it --rm mingrammer/flog:0.3.2 -n 10 >> ./data/weblogs.log; sleep 1; done
    ```

3. Open nifi (localhost:8080/nifi)

4. Add Consume processor (Drag Processor > Search 'kafka' on Filter > Add ConsumerKafka)

    - Kafka Brokers: kafka:29092
    - Topic Name(s): weblogs
    - Group ID: weblogs-consumer-group

5. Add Publish processor

    - Kafka Brokers: kafka:29092
    - Topic Name: weblogs-reproduced

6. Start Consume/Publish processor

7. Check kafka data from kafka console

    ```bash
    docker run -it --network=host edenhill/kafkacat:1.5.0 \
    -b localhost:9092 \
    -G weblog-consumer \
    weblogs
    ```

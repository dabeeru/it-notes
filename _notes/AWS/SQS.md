

- **AWS Message queue**
- Message can contain up to **256 KB**
- Can be used to **queue writes** to the **database**
- Queue types
    - **Standard queues**
        - Unlimited number of transactions per second
        - Guarantee that **transaction** will be **delivered at least ones**
        - Occasionally **one copy of the message** might be delivered **out of order**
        - **Best performance**
    - **FIFO queue**
        - **Fifo enforced**
        - **Exactly\-once processing**
- **Pull\-based**
- Default retention period is 4 days
    Messages can be kept in queue from 1 minute to 14 days
- **Visibility timeout**
    - **after message is delivered is not being visible in the queue for some period of time**, if it is processed due end of that time its deleted, otherwise is visible again
    - Max 12h
- **SQS short polling**  pooling message from the queue, **if queue is empty then empty response is made**
- **SQS long polling**  r**esponse is not returned until message arrives** in the queue

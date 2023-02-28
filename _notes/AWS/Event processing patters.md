Event\-Driven Architecture \- Pub/sub architecture
**Dead\-letter queue ** Queue of letters that hasnt been received by any consumer
- SNS  if message will fail to be delivered to the target is put to DLQ
- **SQS**  when message exceed **maxRecieveCount** \(will be received but not deleted\) then it goes to DLQ
- Lambda  if lambda will raise an error during message processing, it will be **retried twice** and sent to DLQ \(SQS or SNS\)



- **Lambda**
    - **Lambda** takes care about provisioning and managing the servers that you use to run the code, patching os management, scaling
    - Ways of using **lambdas**
        - **Event driven compute service** \(like changes in **S3 buckets** or **DynamoDB**\)
        - Serving **HTTP request** using **AWS API gateway**
    - **Pricing**
        - **Num of requests** \- First 1 million free, 0.20 per million requests after that
        - **Duration** \(time from code begins executing to termination rounded nearest 100ms\) 0.00001667 for every GB/s
    - **Hyperthreading** support
        Lambda scales out \(not up\) \- **1 event = 1 function**
    - **Synchronous triggers**
        - ALB
        - Cognito
        - Lex
        - Alexa
        - API Gateway
        - CloudFront
        - Kinesis Data Firehose
    - Asynchronous triggers
        - S3

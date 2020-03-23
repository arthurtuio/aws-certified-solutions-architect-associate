# Serveless

## History

- Data centre
- IAAS
- PAAS
- Containers
- Serverless

## Lambda

The ultimate abstraction layer in AWS.

### What is?

AWS lambda is a compute service where you can upload your code and create a lambda funciton. AWS Lambda takes care of provision and managing the servers that you use to run the code. You don have to worry about operating systems, patching, scaling, etc. You can use Lamdba in the following ways:
- As an event-driven compute Service where AWS Lambda runs your code in response to events. These events could be changes to data in an Amanzon S3 bucket or an Amazon DynamoDB table.
- As a compute service to run your code in response to HTTP requests using Amazon API gateway or API calls made using AWS SDKs

## Price

- **Number of requests**: first 1 million requests are free, after $0.20 per 1 million requests thereafter.
- **Duration**: calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB-second used.

## Advantags

- No servers
- Continuous Scaling (out, not up)
- Super cheap!

## Exam tips

- Lambda funcitons are independent, 1 event = 1 function
- Lambda functions can trigger other lambda functions, 1 event can trigger X other functions.
- AWS X-ray allows you to debug what is happening
- Lambda can do things globally, you can use it to back up S3 buckets to other buckets, etc.
- Know your triggers

# API Gateway

Is a fully managed service that makes it easy for developers to publish, maintain, monitor and secure APIs at any scale. With a few clicks in the AWS Management Console, you can create an API that acts as a "front door" for applications to access data, business logic or functionality from your back-end services, such as applications running on Amazon Elastic Compute Cloud (Amazon EC2), code running on AWS or any web application

### Type of APIS

- REST APIs (REpresentation State Transfer)
- SOAP APIs (Simple Object Access Protocol)

### What can API Gateway Do?

- Expose HTTPS endpoints to define a RESTful API
- Serverless-ly connect to services like Lambda & Dynamo DB
- Send each API endpoint to a different target
- Run efficiently with low cost
- Scale effortlessly
- Track and control usage by API key
- throttle requests to prevent attacks
- Connect to CloudWatch to log all requests for monitoring



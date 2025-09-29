Fresh Start CC Landing Page

This repo holds the landing page for Fresh Start Cabinet Coating. The frontend is a simple HTML page running on an EC2 instance with Apache. The backend is serverless, using AWS Lambda, DynamoDB, API Gateway, and SNS to handle customer form submissions and notifications.


Live site: https://fresh-start-cc.com


Project Overview

Frontend: Single HTML page hosted on an EC2 t2.micro (Amazon Linux 2, Apache).

Database: DynamoDB table k-customers (partition key: email, streams enabled).


Lambdas:

customer-writer → triggered by API Gateway /submit endpoint, writes form data to DynamoDB.

k-customers-db → triggered by DynamoDB Streams, publishes notifications to SNS.

Notifications: SNS topic k-customers-db-add with an email subscription for new customers.


IAM: Both Lambdas use least-privilege roles. Inline policies handle DynamoDB writes and SNS publishing.


Architecture
Landing Page (EC2) → API Gateway (/submit)
        ↓
Lambda: customer-writer → DynamoDB (k-customers, streams on)
                                      ↓
                      Lambda: k-customers-db → SNS Topic → Email


See infrastructure/architecture.yaml
 for a full breakdown of the resources and policies.



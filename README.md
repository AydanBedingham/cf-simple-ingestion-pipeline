# Simple Data Ingestion Pipeline

CloudFormation template for simple data ingestion pipeline using S3, SQS & Lambda.

## Templates
`ingestion-pipeline.yml` : CloudFormation Template for creating simple data ingestion pipeline resources.

## Architecture
- Defines S3 buckets for Incoming JSON and Incoming CSV files.
- Each bucket has a respective SQS queue which will receive items in response to files being uploaded to the bucket (s3:ObjectCreated:*).
- Lambda functions consume items from the SQS queues, process the files and transfer them to a seperate Curated JSON bucket.
    * JSON files are copied from the Incoming JSON bucket to the Curated JSON bucket.
    * CSV files are read from the Incoming CSV bucket, converted to JSON and transferred to the Curated JSON bucket.
    * SQS items are processed in groups with item processing failures handled using partial batch responses. Repeated failures will reuslt in the SQS item being transferred to a corresponding Dead Letter Queue (DLQ).
![Screenshot](images/simple-ingestion-pipeline.svg?raw=true)

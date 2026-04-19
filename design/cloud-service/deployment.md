# Deployment

> *Stub.*

## Dev
- Docker Compose: app + Postgres (pgvector) + LocalStack (optional, for S3/Textract/Polly mocking)
- See [`../../infra/docker/`](../../infra/docker/)

## Prod (v1)
- Single EC2 (t4g.small or similar) running the Spring Boot app
- RDS Postgres with pgvector
- S3 bucket for files
- IAM role on EC2 with scoped access to Bedrock, Transcribe, Polly, Textract, S3
- CloudWatch logs

## IaC choice: open
Decide between AWS CDK (Java-native, infra-as-app-code) and Terraform (de-facto standard, easier for OSS contributors to grok). See [`../07-open-questions.md`](../07-open-questions.md) #3.

# AWS Production Infra

**Status:** Not yet populated.

Will contain Infrastructure-as-Code (CDK or Terraform — decision open; see [`../../design/07-open-questions.md`](../../design/07-open-questions.md) #3) for:

- EC2 instance (single node, v1)
- RDS Postgres with pgvector
- S3 bucket for files
- IAM role with scoped access to Bedrock, Transcribe, Polly, Textract, S3
- CloudWatch logs
- Security groups

Design context: [`../../design/cloud-service/deployment.md`](../../design/cloud-service/deployment.md).

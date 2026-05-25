---
name: aws-docs-reader
description: Fetch AWS documentation as clean markdown using llms.txt index and content negotiation. Use when you need to look up AWS service documentation, API references, or guides. Works for all AWS services without a browser.
metadata:
  author: "gilinachum"
  version: "1.0"
---

# AWS Docs Reader

AWS documentation supports direct markdown access. Use this instead of rendering HTML pages.

## How to Fetch AWS Docs

### 1. Find the right page — llms.txt index

Every AWS guide has an `llms.txt` file listing all pages:

```
https://docs.aws.amazon.com/{service-path}/llms.txt
```

Examples:
- `https://docs.aws.amazon.com/bedrock/latest/userguide/llms.txt`
- `https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/llms.txt`
- `https://docs.aws.amazon.com/AmazonS3/latest/userguide/llms.txt`
- `https://docs.aws.amazon.com/lambda/latest/dg/llms.txt`
- `https://docs.aws.amazon.com/AmazonECS/latest/developerguide/llms.txt`
- `https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/llms.txt`
- `https://docs.aws.amazon.com/cdk/v2/guide/llms.txt`
- `https://docs.aws.amazon.com/bedrock/latest/APIReference/llms.txt`
- `https://docs.aws.amazon.com/cli/latest/userguide/llms.txt`

### 2. Fetch a specific page — two methods

**Method A: `.md` suffix** (preferred)

Replace `.html` with `.md` in any page URL from the llms.txt index:
```
https://docs.aws.amazon.com/bedrock/latest/userguide/kb-test-config.md
```

**Method B: `Accept: text/markdown` header**

Send the header on a standard `.html` URL:
```bash
curl -H "Accept: text/markdown" https://docs.aws.amazon.com/bedrock/latest/userguide/kb-test-config.html
```

Both return clean markdown with `Content-Type: text/markdown`.

## Important: Case Sensitivity

The path casing varies between services and **must match exactly**:

| Service | Path |
|---------|------|
| Bedrock | `bedrock/latest/userguide/` |
| EC2 | `AWSEC2/latest/UserGuide/` |
| S3 | `AmazonS3/latest/userguide/` |
| Lambda | `lambda/latest/dg/` |
| ECS | `AmazonECS/latest/developerguide/` |
| DynamoDB | `amazondynamodb/latest/developerguide/` |
| IAM | `IAM/latest/UserGuide/` |
| CloudFormation | `AWSCloudFormation/latest/UserGuide/` |
| CDK | `cdk/v2/guide/` |
| SageMaker | `sagemaker/latest/dg/` |

Wrong casing returns a 404. When unsure, check the canonical URL on the AWS docs site or fetch the root `llms.txt`.

## Workflow

1. Identify which guide you need (userguide, API reference, developer guide)
2. Fetch `llms.txt` for that guide to discover page slugs
3. Fetch the specific page with `.md` suffix
4. If you don't know the exact service path, try `https://docs.aws.amazon.com/llms.txt` (root index)

## What NOT to do

- Don't fetch the `.html` URLs without the Accept header — they return an SPA shell that requires JavaScript
- Don't guess the path casing — verify it
- Don't use headless browsers for AWS docs — markdown access is faster and cleaner

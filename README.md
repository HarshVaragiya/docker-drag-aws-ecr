# docker-drag-aws-ecr

This repository contains Python scripts for interacting with AWS ECR without needing the Docker client itself.

It relies on the Docker registry API supported by AWS ECR [HTTPS API v2](https://docs.docker.com/registry/spec/api/) [AWS DOCS](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html)

## Pull a Docker image in HTTPS

`python3 docker_pull.py <aws-account-id>.dkr.ecr.<repository-region>.amazonaws.com/<image-name>:<image-tag>`

`python3 docker_pull.py 123456789123.dkr.ecr.us-east-1.amazonaws.com/my-test-image:1`

## Well known bugs
2 open bugs which shouldn't affect the efficiency of the script nor the pulled image:
- Unicode content (for example `\u003c`) gets automatically decoded by `json.loads()` which differs from the original Docker client behaviour (`\u003c` should not be decoded when creating the TAR file). This is due to the json Python library automatically converting string to unicode.
- Fake layers ID are not calculated the same way than Docker client does (I don't know yet how layer hashes are generated, but it seems deterministic and based on the client)

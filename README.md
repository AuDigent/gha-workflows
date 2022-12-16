# gha-workflows
Reusable Github Action Workflows

## Examples:

### Workflows

- `aws-deploy-ecs.yml`:
```yaml
name: Try workflow
on:
  push:
    branches:
      - try-workflow

jobs:
  try-workflow:
    uses: AuDigent/gha-workflows/.github/workflows/aws-deploy-ecs.yml@0.2.1
    with:
      ecr-repository-name: your-repo
      docker-image-tag: ${{ github.sha }}
      docker-build-platforms: linux/arm64
      environment: dev
      ecs-cluster: your-cluster
      ecs-service: your_service
      container-name: some-container
      aws-region: us-east-1
    secrets: inherit
```

### Actions

- `build-and-push-to-ecr`:
```yaml
- uses: actions/checkout@v3
- name: Push Image to ECR
  uses: AuDigent/gha-workflows/.github/actions/build-and-push-to-ecr@0.2.1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1
    docker-image-tag: ${{ github.sha }}
    ecr-repository-name: my-repo
```

- `deploy-to-ecs`:
```yaml
- uses: actions/checkout@v3
- name: Deploy to ECS
  uses: AuDigent/gha-workflows/.github/actions/deploy-to-ecs@0.2.1
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: us-east-1
    ecs-cluster: your-cluster
    ecs-service: your_service
    docker-image: myregistry/myimage:tag
    container-name: my_container
```

- `get-codeartifact-token`:
```yaml
- uses: actions/checkout@v3
- name: Get CodeArtifact Token
  id: token
  uses: AuDigent/gha-workflows/.github/actions/get-codeartifact-token@0.4.0
  with:
    domain: my-domain
    domain-owner: 1111111   # Your AWS Account ID
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
- uses: actions/setup-python@v4
  with:
    python-version: '3.9'
    cache: 'pipenv'
- name: install dependencies
  env:
    CODEARTIFACT_AUTH_TOKEN: ${{ steps.token.outputs.token }}
  run: |
    pip install pipenv
    pipenv install --dev --ignore-pipfile
```

Example of pipenv config:
```
[[source]]
name = "aws"
url = "https://aws:${CODEARTIFACT_AUTH_TOKEN}@xxxxxx.d.codeartifact.us-east-1.amazonaws.com/pypi/xxxxx/simple/"
verify_ssl = true

[packages]
your-package = { version = "~=1.0", index = "aws" }
```

Example of IAM Policies to grant access to the repositories available [here](https://docs.aws.amazon.com/codeartifact/latest/ug/auth-and-access-control-iam-identity-based-access-control.html)
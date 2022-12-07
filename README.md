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

### Actions

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

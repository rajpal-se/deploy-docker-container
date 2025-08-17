# Deploy Docker Container GitHub Action

This GitHub Action simplifies the deployment of Docker containers using AWS Elastic Container Registry (ECR) images. It provides configurable options for managing environment variables, network settings, and additional Docker run options.

---

## Features

- Pull Docker images from AWS ECR.
- Run containers with customizable settings, such as environment variables, network configuration, and additional Docker run options.
- Stop and remove existing containers before deploying a new one.
- Secure integration with AWS ECR and SSH-based remote host connections.

---

## Inputs

### Required Inputs

| Name                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| `aws-account-id`     | AWS account ID.                                 |
| `aws-ecr-region`     | AWS ECR region.                                 |
| `aws-ecr-repository` | The AWS ECR repository name.                    |
| `app-container-name` | Name of the Docker container.                   |
| `app-port-mapping`   | Port mapping for the container (e.g., `80:80`). |

### Optional Inputs

| Name                     | Description                          | Default          |
| ------------------------ | ------------------------------------ | ---------------- |
| `host`                   | Host for SSH connection.             | `github-actions` |
| `ecr-image-tag`          | ECR image tag to be deployed.        | `latest`         |
| `app-network`            | Network for the Docker container.    | `vps`            |
| `app-container-ip`       | IP address for the Docker container. | N/A              |
| `app-env-file`           | Environment file data.               | N/A              |
| `app-docker-run-options` | Additional Docker run options.       | N/A              |

---

## Usage

### Workflow Example

Here is a sample workflow that demonstrates how to use the `Deploy Docker Container` action:

```yaml
name: "Deploy"

on:
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Configure SSH Credentials
        uses: rajpal-se/configure-ssh-credentials@v1
        with:
          key: ${{ secrets.SSH_KEY }}
          hostname: ${{ secrets.HOSTNAME }}

      - name: Deploy Docker Container
        uses: rajpal-se/deploy-docker-container@latest
        with:
          aws-account-id: ${{ secrets.AWS_ACCOUNT_ID }}
          aws-ecr-region: ${{ secrets.AWS_ECR_REGION }}
          aws-ecr-repository: ${{ secrets.AWS_ECR_REPOSITORY }}
          ecr-image-tag: latest
          app-container-name: ${{ secrets.APP_CONTAINER_NAME }}
          app-port-mapping: ${{ secrets.APP_PORT_MAPPING }}
          app-env-file: ${{ secrets.APP_ENV_FILE }}
          app-docker-run-options: "--label MyLabel"
```

---

## Input Details

### `app-env-file`

- **Description**: Content of the environment file to be applied to the container.
- **Usage**: If specified, the content will be written to a temporary file and passed to the Docker container using the `--env-file` option.

### `app-docker-run-options`

- **Description**: Additional Docker run options such as labels, volumes, or resource limits.
- **Example**: `--label MyLabel --rm -v /host/path:/container/path`

---

## Environment Setup

### AWS Credentials

Ensure the following secrets are set in your repository:

- `AWS_ACCOUNT_ID`: AWS Account ID.
- `AWS_ECR_REGION`: AWS region for ECR.
- `AWS_ECR_REPOSITORY`: Name of the ECR repository.

### SSH Configuration

To deploy on a remote server, configure SSH credentials using the `rajpal-se/configure-ssh-credentials` action.

---

## Debugging and Logs

For debugging purposes, ensure verbose logs are enabled in your workflow by setting the `ACTIONS_RUNNER_DEBUG` secret to `true`.

---

## Contributions

Contributions are welcome! Please open an issue or submit a pull request to improve this action.

---

## License

This GitHub Action is licensed under the MIT License.

# NEAR AI: A Comprehensive Platform for AI Agent Development and Deployment

## Project Overview

This project is a comprehensive AI agent platform designed to facilitate the creation, management, and deployment of intelligent agents for various tasks. The codebase provides a robust infrastructure for building, running, and interacting with AI agents, integrating with modern AI models and services.

### Main Purpose and Problems Solved
The primary purpose of this platform is to simplify the development and deployment of AI agents that can perform complex tasks autonomously or collaboratively. It addresses challenges such as agent orchestration, secure communication with AI services, and integration with diverse data sources and APIs. By providing a unified framework, it reduces the complexity of managing AI workflows and enables developers to focus on creating innovative solutions.

### Key Features and Benefits
- **Agent Creation and Management**: Tools and APIs to easily create, configure, and manage AI agents with customizable behaviors and capabilities.
- **Integration with AI Models**: Seamless connectivity to various AI inference services for tasks like natural language processing, image generation, and embeddings.
- **Secure Environment**: Built-in mechanisms for secure authentication and communication, ensuring data privacy and integrity.
- **Scalable Deployment**: Support for deploying agents in different environments, including local, cloud (AWS), and containerized setups using Docker.
- **Rich API and CLI**: Comprehensive API endpoints and a command-line interface for interacting with the platform, automating tasks, and integrating with external systems.
- **Vector Stores and Data Handling**: Advanced capabilities for managing vector stores, embeddings, and datasets to support Retrieval-Augmented Generation (RAG) and other data-intensive applications.
- **Community and Collaboration**: Features for sharing agents, forking projects, and collaborating through a hub interface, fostering a community-driven ecosystem.

This platform empowers developers and organizations to leverage AI agents for automation, decision-making, and enhanced user interactions, providing a flexible and powerful toolset for modern AI-driven applications.

## Getting Started, Installation, and Setup

### Quick Start Guide

Welcome to NEAR AI, a distributed system for building, deploying, and managing AI agents. This quick start guide will help you get up and running with the core components. Detailed installation instructions are provided in the sections below.

1. **Install NEAR AI CLI**:
   - If you have Python 3.11 installed, you can quickly install the NEAR AI CLI using pip:
     ```bash
     python3 -m pip install nearai
     ```
   - Verify the installation:
     ```bash
     nearai version
     ```

2. **Login to NEAR AI**:
   - Authenticate with your NEAR Account. If you don't have one, create a free account with [Meteor Wallet](https://wallet.meteorwallet.app).
     ```bash
     nearai login
     ```

3. **Create and Run an Agent**:
   - Create a new AI agent:
     ```bash
     nearai agent create
     ```
   - Run the agent locally for testing:
     ```bash
     nearai agent interactive
     ```

4. **Deploy Your Agent**:
   - Upload your agent to the [NEAR AI Developer Hub](https://hub.near.ai):
     ```bash
     nearai registry upload <path-to-agent>
     ```

### Installation and Setup

#### Prerequisites
- **Python 3.11**: Ensure you have Python 3.11 installed (3.12+ is currently not supported). Download it from [python.org](https://www.python.org/downloads/).
- **Git**: Required for cloning the repository. Install it following [Git guides](https://github.com/git-guides/install-git).
- **Docker**: Necessary for local agent testing or development setups. Install Docker by following the instructions at [docker.com](https://docs.docker.com/get-docker/).

#### Installation Methods

##### Method 1: Install via pip (Recommended for Users)
This method is ideal for users who want to quickly get started with the NEAR AI CLI.

```bash
python3 -m pip install nearai
```

Verify the installation:

```bash
nearai version
```

##### Method 2: Local Installation (For Developers)
This method is suitable for developers who want to work on the NEAR AI codebase or need a local development environment.

1. Clone the repository and set up a virtual environment:
   ```bash
git clone git@github.com:nearai/nearai.git && cd nearai && ./install.sh
   ```
   The `install.sh` script will:
   - Create a virtual environment in `.venv`
   - Activate the environment
   - Upgrade pip
   - Install the project in editable mode
   - Verify the installation by running `nearai version`

2. Alternatively, use `uv` for dependency management in a virtual environment:
   ```bash
python3 -m uv sync
uv run nearai version
   ```

3. Or install directly with pip in editable mode:
   ```bash
python3 -m pip install -e .
   ```

   Verify the installation:
   ```bash
nearai version
   ```

#### Running in Development

For local development and testing of AI agents, you can use Docker to set up the necessary services. The provided `docker-compose.yaml` configures a local environment with the following components:
- **Runner Service**: Executes AI agents locally.
- **Database**: Uses SingleStore for data storage.
- **Datadog Agent**: Optional monitoring and logging service.

To set up and run the development environment:

1. Ensure Docker and Docker Compose are installed.
2. Create a `.env` file in the root directory with necessary environment variables (refer to `.env_example` for required variables like `RUNNER_API_KEY`).
3. Run the following command to start the services:
   ```bash
docker-compose up --build
   ```
   This will build the runner image and start all services defined in `docker-compose.yaml`.
4. Access the runner service on port `9009` for local agent execution.

#### Building for Production

To prepare your agent or project for production deployment:

1. **Deploy to NEAR AI Hub**:
   - After testing your agent locally, deploy it to the NEAR AI Developer Hub for public or private access:
     ```bash
nearai registry upload <path-to-agent>
     ```

2. **AWS Runner Setup** (Optional):
   - For scalable execution on AWS Lambda, refer to the instructions in [aws_runner/README.md](./aws_runner/README.md) to configure and deploy your agents.

#### Platform-Specific Instructions

- **Linux/macOS**: The installation scripts and Docker setups provided should work seamlessly on Linux and macOS systems. Ensure you have the required permissions to install packages and run Docker.
- **Windows**: While not explicitly tested, you can use WSL2 (Windows Subsystem for Linux) with Docker Desktop to replicate a Linux environment for development. Install Python, Git, and Docker within WSL2 for compatibility.

#### Updating NEAR AI

To update your local installation to the latest version:

```bash
cd nearai
git pull
python3 -m pip install -e .  # Reinstall if dependencies have changed
```

For more detailed documentation, visit the [Official Documentation](https://docs.near.ai) and the [Agent Development Guide](https://docs.near.ai/agents/quickstart).

## Authentication

This section describes the authentication mechanism used in the application for securing API endpoints.

### Authentication Mechanism

The application uses a custom authentication system based on signed messages and nonces with the NEAR protocol. Authentication is managed through a Bearer token scheme where the token contains a JSON payload with necessary authentication data. This data includes the account ID, public key, signature, nonce, and other relevant fields necessary for validation.

#### How Authentication Works

- **Token Structure**: Authentication tokens are passed in the `Authorization` header as Bearer tokens. The token itself is a JSON object containing fields like `account_id`, `public_key`, `signature`, `nonce`, and `message`. These fields are used to verify the authenticity of the request.
- **Signature Verification**: Upon receiving a token, the system verifies the signature using the provided public key against the message and nonce to ensure the request is from a legitimate source.
- **Nonce Validation**: Nonces are checked to prevent replay attacks. If a nonce is found to be revoked or already used, the authentication fails.
- **Delegation**: The system supports delegated actions where one account can act on behalf of another if permissions are granted. This is verified through a database check for delegation records.
- **Token Validation Flow**: 
  1. The token is extracted from the Authorization header.
  2. The JSON payload is parsed to validate its structure and content.
  3. Signature is verified using the NEAR protocol's cryptographic functions.
  4. If delegation is involved, permissions are checked.
  5. Nonce is validated to ensure it hasn't been revoked or used previously.

#### Example Authentication Header

```plaintext
Authorization: Bearer {"account_id": "user.near", "public_key": "ed25519:abc...", "signature": "xyz...", "nonce": "base64nonce...", "message": "login request", "recipient": "ai.near"}
```

This header must be included in requests to protected endpoints. The token's JSON structure must be valid and verifiable through the defined authentication process. If any step fails, the request will be rejected with an appropriate HTTP status code (e.g., 401 Unauthorized or 403 Forbidden).

### Security Considerations

- Ensure that tokens are transmitted over secure channels (HTTPS) to prevent interception.
- Regularly monitor and manage nonce usage to prevent unauthorized access through replay attacks.
- When using delegation, ensure that permissions are tightly controlled and expiration dates are set appropriately to limit potential misuse.

## Deployment

### Docker Deployment

The repository provides several Docker configurations for deploying services related to the NearAI project. Below are the primary methods for deploying using Docker:

#### Development Environment
- **Dockerfile**: Located at `.docker/Dockerfile.gpu.dev`, this image is tailored for development purposes with GPU support.
- **Compose File**: Use `.docker/compose.yml` to set up a development environment. It mounts the necessary volumes for code and configuration, and allocates GPU resources.
- **Command**: Run `docker-compose -f .docker/compose.yml up` to start the development environment with GPU support for up to 8 NVIDIA GPUs.

#### Worker and Scheduler Deployment
- **Dockerfile**: Workers and schedulers are built using `.docker/Dockerfile.worker` and `.docker/Dockerfile.scheduler` respectively.
- **Compose File**: The `.docker/compose-worker.yml` file orchestrates the deployment of worker nodes, a scheduler, and a proxy service with specific environment variables and security settings.
- **Environment Configuration**: Ensure environment variables such as `NEARAIWORKER_ACCOUNT_ID`, `NEARAIWORKER_SIGNATURE`, and others are set for authentication and configuration. These can be provided via an `.env` file or directly in the environment.
- **Scaling Considerations**: The worker service is configured with resource limits (e.g., CPU: 0.50, Memory: 128M) and GPU reservations (1 NVIDIA GPU). Adjust these in the `deploy.resources` section of the compose file based on your hardware capabilities and workload requirements.
- **Command**: Use `docker-compose -f .docker/compose-worker.yml up` to deploy the worker and scheduler services.

#### Local Runner Deployment
- **Dockerfile**: Located at `aws_runner/py_runner/Dockerfile`, this is used for deploying a local runner service.
- **Compose File**: Use `docker-compose.yaml` at the root of the repository for a local runner setup. It maps necessary ports and volumes for registry data.
- **Environment Configuration**: Set `RUNNER_API_KEY` and Datadog-related variables in an `.env` file or directly in the environment for monitoring and authentication.
- **Command**: Run `docker-compose up` to start the local runner service.

### AWS Lambda Deployment

For deploying agent runners on AWS Lambda, the repository includes a deployment script:
- **Script**: Located at `aws_runner/deploy.sh`, this script automates the build, push, and update process for Docker images to AWS Elastic Container Registry (ECR) and AWS Lambda.
- **Frameworks and Environments**: The script supports multiple frameworks (minimal, langgraph-0-1-4, langgraph-0-2-26, standard, agentkit) and environments (staging, production).
- **Usage**: 
  - Deploy a specific framework and environment: `FRAMEWORK=minimal ENV=staging aws_runner/deploy.sh`
  - Deploy all frameworks to all environments: `aws_runner/deploy.sh all`
- **Requirements**: Ensure AWS CLI is configured with appropriate credentials and region settings (us-east-2). The script logs into ECR, builds and tags the Docker image, pushes it to ECR, and updates the Lambda function.

### Environment Configuration

- **General**: Environment variables are critical for authentication, monitoring, and service configuration. Use `.env` files or set them directly in your deployment environment.
- **Datadog**: For monitoring, configure Datadog API keys and other settings as seen in the Docker compose files (`DD_API_KEY`, `DD_ENV`, etc.).
- **Security**: Worker services are deployed with restricted privileges (`no-new-privileges:true`) and specific capabilities dropped for security. Adjust as necessary based on your security policies.

### Scaling Considerations

- **Docker**: Adjust resource limits and reservations in the compose files based on workload and hardware availability. For GPU-intensive tasks, ensure adequate GPU allocation.
- **AWS Lambda**: Consider the concurrency limits and memory configurations for Lambda functions when deploying multiple frameworks or in high-traffic environments. Monitor performance and adjust using AWS CloudWatch metrics.

Ensure that you review and adapt these configurations to match your specific deployment needs and infrastructure constraints.

## Project Structure

This section outlines the structure of the repository, detailing the purpose of key directories and files to help you navigate the codebase effectively.

### Directory Structure

- **.docker/**: Contains Dockerfiles and compose configurations for various components like agents, workers, schedulers, and development environments. Key files include `Dockerfile.agent_runner` for agent execution and `compose.yml` for orchestrating multiple services.
- **aws_runner/**: Includes scripts and configurations for deploying and running agents on AWS. Notable files are `deploy.sh` for deployment automation and subdirectories like `py_runner/` and `ts_runner/` for Python and TypeScript runtime environments.
- **docs/**: Houses comprehensive documentation for the project. It includes guides on agents (`agents/`), tutorials on retrieval-augmented generation (`tutorials/rag/`), and API documentation (`api.md`). Assets like images and scripts are also stored here.
- **hub/**: Central backend application directory. It contains the API implementation under `api/v1/` with endpoints for agents, threads, and more. The `demo/` subdirectory includes a Next.js frontend application for user interaction. Database migrations are managed via `alembic/`.
- **nearai/**: Core library for interacting with the platform. It includes modules for agent creation (`agents/`), CLI tools (`cli.py`), API clients (`openapi_client/`), and various utilities for benchmarks, datasets, and model interactions.
- **ts_runner/**: TypeScript-based agent runtime environment. Key components include `ts_agent/` for agent logic and `ts_agent_runner/` for executing TypeScript agents with necessary SDKs and configurations.
- **worker/**: Contains worker scripts for background tasks and processing. Entry points like `__main__.py` are used to run worker processes.
- **etc/**: Stores miscellaneous configuration files and scripts, such as `finetune/` for model fine-tuning configurations and setup scripts for server environments.
- **scripts/**: Utility scripts for development tasks like building documentation (`build_mkdocs.sh`), linting (`lint_check.sh`), and generating API clients (`generate_client.sh`).

### Key Files

- **docker-compose.yaml**: Main Docker Compose configuration for local development and testing.
- **pyproject.toml**: Defines project dependencies and metadata for Python components.
- **uv.lock**: Lock file for dependency versions, ensuring reproducibility.
- **README.md**: The main documentation entry point for an overview of the project.
- **CHANGELOG.md**: Tracks changes and updates across versions.
- **LICENSE**: Specifies the legal terms under which the project is distributed.

This structure facilitates a modular approach, separating concerns between backend services, frontend interfaces, agent runtimes, and documentation, making it easier to develop, deploy, and maintain the system.

## Technologies Used

This project leverages a variety of modern technologies, frameworks, libraries, SDKs, and tools to build a robust and scalable AI agent platform. Below is a list of the major technologies used:

### Frameworks and Libraries
- **Python**: The core programming language used for backend development and AI agent logic.
- **FastAPI**: A modern, fast (high-performance) web framework for building APIs with Python, used in the hub for serving API endpoints.
- **Alembic**: A lightweight database migration tool for SQLAlchemy, used for managing database schema changes in the hub.
- **SQLAlchemy**: An ORM (Object-Relational Mapping) library for Python, used for database interactions in the hub.
- **Next.js**: A React framework for building server-side rendered and static web applications, used for the frontend demo application.
- **React**: A JavaScript library for building user interfaces, utilized in the frontend demo for creating dynamic components.
- **TypeScript**: A superset of JavaScript that adds static typing, used for developing type-safe code in the frontend and TypeScript agent runner.
- **tRPC**: A lightweight, type-safe RPC framework for building end-to-end typesafe APIs, used in the frontend for communication with backend services.

### SDKs and Tools
- **Docker**: Used for containerization, with multiple Dockerfiles for different components like agent runners, workers, and development environments.
- **Docker Compose**: A tool for defining and running multi-container Docker applications, used for local development and testing setups.
- **NEAR Protocol SDK**: Custom SDK components for interacting with the NEAR blockchain, including secure clients and signing utilities for authentication and transactions.
- **OpenAI API Clients**: Custom secure clients for interacting with OpenAI's APIs for inference tasks like chat completions and embeddings.
- **GitHub Actions**: Used for CI/CD workflows, including testing, building, and deployment processes.
- **ESLint & Prettier**: Tools for linting and formatting code to maintain consistency across Python, JavaScript, and TypeScript codebases.
- **Streamlit**: A Python library for creating interactive web apps, used for inspection tools in the project.
- **TensorBoard**: A visualization toolkit for machine learning experimentation, integrated for feeding and visualizing training data.

### Databases and Storage
- **PostgreSQL**: Implied as the database used with SQLAlchemy and Alembic for managing application data in the hub.
- **Vector Stores**: Custom implementations for managing embeddings and vector data, crucial for AI and machine learning tasks.

### Other Technologies
- **NGINX**: Configurable as a reverse proxy or load balancer for serving the hub application in production environments.
- **Supervisor**: A process control system used for managing server processes in production setups.
- **AWS Lambda**: Supported through custom runners for deploying agent logic in serverless environments.

These technologies collectively enable the development, deployment, and management of AI agents, inference services, and user interfaces within this repository.

## Additional Notes

### Security Considerations

- **Authentication and Authorization**: This project implements secure authentication mechanisms as seen in the codebase. Users should ensure that API keys and secrets are properly managed and not exposed in public repositories or client-side code. Utilize the provided authentication endpoints and secure clients for safe interactions with the platform.
- **Data Privacy**: When deploying agents or utilizing models that process sensitive data, ensure compliance with relevant data protection regulations. Be cautious when handling user data through threads and messages.

### Performance Optimization

- **Efficient Resource Usage**: Given the presence of Docker configurations and AWS runner setups, optimize resource allocation based on workload. Adjust Docker compose settings and AWS configurations to balance performance and cost.
- **Caching**: The codebase includes caching mechanisms (as seen in `nearai/shared/cache.py`). Leverage these to improve response times for frequently accessed data or computations.

### Extensibility

- **Agent Development**: The repository supports creating and running custom AI agents. Explore the `nearai/agents` directory for templates and examples to extend functionality with your own agent implementations.
- **Integration Points**: With APIs for delegation, vector stores, and more, the system is designed for extensibility. Check the API documentation and examples in the `hub/examples` folder for integration ideas.

### Troubleshooting

- **Logs and Debugging**: Utilize the logging facilities available in `nearai/log.py` and API endpoints for logs to diagnose issues. The system supports detailed logging which can be enabled as needed.
- **Common Issues**: If encountering problems with agent execution or API connectivity, verify environment variables and configuration settings in `.env` files or through the CLI tools. Ensure Docker containers are properly networked if using containerized deployments.

### Community and Support

- **Contributing**: Contributions are welcome! Check the guidelines in `docs/contributing.md` for how to submit issues, feature requests, or pull requests.
- **Examples and Tutorials**: The repository contains numerous examples under `hub/examples` and tutorials in the `docs/tutorials` directory to help users get started and understand advanced concepts like RAG (Retrieval-Augmented Generation).

### Known Limitations

- **Scalability**: While the system supports distributed setups via AWS and Docker, certain configurations may have scalability limits depending on the chosen deployment model. Test thoroughly for your specific use case.
- **Feature Maturity**: Some components, especially newer integrations or experimental features in the codebase, may not be fully stable. Refer to `CHANGELOG.md` for version-specific notes and updates.

## Contributing

Everyone is welcome to contribute to this project, and we value every contribution. While code contributions are significant, they are not the only way to help the community. Answering questions, assisting others, and improving documentation are equally valuable.

You can also support the project by spreading the word! Reference the library in blog posts about the projects it has enabled, or simply star the repository on GitHub to show your appreciation.

### Ways to Contribute

There are several ways you can contribute to the project:

- **Fixing Outstanding Issues**: If you identify an issue with the existing code and have a solution, feel free to open a Pull Request (PR) with your fix.
- **Submitting Bug Reports or Feature Requests**: Help improve the project by reporting bugs or suggesting new features. Ensure the issue hasn't already been reported by searching on GitHub under Issues.
  - **Bug Reports**: Include details such as what you did, what you expected to happen, what happened instead, your OS and software versions, a reproducible code snippet, the full traceback if applicable, and any additional information like screenshots.
  - **Feature Requests**: Describe the motivation behind the feature, provide detailed explanations, include a code snippet demonstrating its usage, and link to any relevant papers if applicable.
- **Contributing Documentation**: If you notice errors or omissions in the documentation, open an issue describing the problem or submit a PR with corrections. To preview documentation changes locally, install dependencies with `pip install -e ".[docs]"` or `uv sync --group docs`, and use `mkdocs serve` to test changes.
- **Creating a Pull Request**: Before coding, check existing PRs or issues to avoid duplication. Follow these steps to contribute:
  1. Fork the repository by clicking the 'Fork' button on the repository's GitHub page.
  2. Clone your fork to your local machine and add the base repository as a remote:
     ```bash
     git clone git@github.com:<your Github handle>/nearai.git
     cd nearai
     git remote add upstream https://github.com/nearai/nearai.git
     ```
  3. Create a new branch for your changes:
     ```bash
     git checkout -b a-descriptive-name-for-my-changes
     ```
  4. Set up a development environment as outlined in the repository's README.
  5. Develop your features on your branch, ensuring the code meets the project's standards.
  6. Format and type-check your code using the provided scripts:
     ```bash
     ./scripts/format_check.sh
     ./scripts/lint_check.sh
     ./scripts/type_check.sh
     ```
  7. Commit your changes with clear, descriptive commit messages:
     ```bash
     git add modified_file.py
     git commit
     ```
  8. Keep your branch up to date by rebasing on `upstream/main` before opening a PR:
     ```bash
     git fetch upstream
     git rebase upstream/main
     ```
  9. Push your changes to your fork:
     ```bash
     git push -u origin a-descriptive-name-for-my-changes
     ```
  10. Open a Pull Request on GitHub, ensuring it meets the checklist criteria below.

### Pull Request Guidelines

- **Title and Description**: The PR title should summarize your contribution. Mention any related issue numbers in the description.
- **Work in Progress**: Prefix the title with `[WIP]` if the PR is not yet ready for merging.
- **Media Files**: Avoid adding images, videos, or other non-text files that increase repository size. Reference them via URL instead.

### Code Style and Testing Requirements

- **Formatting and Type Checking**: The project uses `ruff` for formatting and `mypy` for type checking to maintain code consistency. Run the provided scripts (`format_check.sh`, `lint_check.sh`, `type_check.sh`) to ensure your code adheres to these standards before submitting a PR.
- **Testing**: While specific testing requirements are not detailed in this section, ensure that your changes do not break existing functionality and include relevant tests if applicable.

### Additional Notes

- If maintainers request changes to your PR, work on those changes in your local branch and push them to your fork. They will automatically update in the PR.
- For syncing a forked repository with the upstream main branch, avoid unnecessary PRs and notifications by merging directly into your forked main when possible, or use `git pull --squash --no-commit` if a PR is necessary.

## License

This project is licensed under the MIT License. For more details, please see the [LICENSE](./LICENSE) file.
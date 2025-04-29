# NEAR AI: A Decentralized Platform for Open-Source Artificial General Intelligence (AGI) Development

## Project Overview

NEAR AI is a comprehensive distributed system designed to revolutionize AI agent development, deployment, and management. The platform aims to create an open-source, user-owned Artificial General Intelligence (AGI) ecosystem that empowers developers to build, share, and run intelligent agents with unprecedented flexibility and transparency.

### Core Purpose

The project addresses critical challenges in AI development by providing a unified platform that:
- Enables seamless creation and deployment of AI agents
- Offers robust infrastructure for distributed AI computations
- Supports secure and isolated agent execution environments
- Facilitates collaborative AI research and development

### Key Features

#### Distributed Agent Infrastructure
- Decentralized agent registry and management system
- Multiple execution environments (AWS Lambda, TEE Runner, Local Runner)
- Comprehensive agent lifecycle support from creation to deployment

#### Advanced Execution Capabilities
- Environment isolation for agent runtime
- Confidential computing support
- Distributed job scheduling and execution
- Model fine-tuning infrastructure

#### Developer-Friendly Ecosystem
- Command-line interface for agent management
- Easy agent creation and deployment workflows
- Integrated tools for agent development
- Flexible framework support

#### Security and Transparency
- Open-source architecture
- User-owned AI agent platform
- Secure execution environments
- Transparent agent development process

### Key Benefits
- Democratizes AI agent development
- Reduces complexity of AI infrastructure
- Provides scalable and flexible AI agent deployment
- Supports collaborative AI research
- Enables community-driven AI innovation

## Getting Started, Installation, and Setup

### Prerequisites

Before you begin, ensure you have the following:

- **Python**: Version 3.9-3.11 (3.12+ not supported)
- **Operating System**: Linux, macOS, or Windows with Python support
- **Optional**: Docker (recommended for local agent testing)

### Quick Installation

#### Using pip (Recommended)

Install the latest version of NEAR AI directly from PyPI:

```bash
python3 -m pip install nearai
```

#### Local Development Installation

Clone the repository and install in editable mode:

```bash
# Clone the repository
git clone https://github.com/nearai/nearai.git
cd nearai

# Option 1: Using install script
./install.sh

# Option 2: Using pip
python3 -m pip install -e .
```

### Verify Installation

Confirm the installation by checking the version:

```bash
nearai version
```

### Development Environment Setup

For developers or advanced users, we recommend using a virtual environment:

```bash
# Create a virtual environment
python3 -m venv nearai_env
source nearai_env/bin/activate  # On Windows, use `nearai_env\Scripts\activate`

# Install NEAR AI
python3 -m pip install nearai
```

### Optional Dependencies

NEAR AI supports various optional feature sets. Install them using pip with extras:

```bash
# Install with hub features
python3 -m pip install "nearai[hub]"

# Install with machine learning support
python3 -m pip install "nearai[torch]"

# Install development tools
python3 -m pip install "nearai[dev]"
```

### First-Time Setup

1. **Login to NEAR AI**:
   ```bash
   nearai login
   ```
   - Recommended: Create a free account with [Meteor Wallet](https://wallet.meteorwallet.app)

2. **Create Your First Agent**:
   ```bash
   nearai agent create
   ```

3. **Run Agent Interactively**:
   ```bash
   nearai agent interactive
   ```

### Updating NEAR AI

To update to the latest version:

```bash
# Update repository
cd nearai
git pull

# Reinstall with updated dependencies
python3 -m pip install -e .
```

### Troubleshooting

- Ensure Python version is between 3.9 and 3.11
- Check that all dependencies are correctly installed
- Verify network connectivity for NEAR AI services
- Consult [NEAR AI Documentation](https://docs.near.ai) for detailed guidance

## Core Concepts

The NEAR AI platform is built around several core conceptual components that enable flexible and powerful AI agent development and execution:

### Agent Architecture

Agents are the primary building blocks of the platform, representing autonomous computational units with the following key characteristics:

- **Flexible Language Support**: Agents can be implemented in both Python and TypeScript
- **Modular Design**: Each agent is stored in a structured registry with a versioned namespace
- **Environment Interaction**: Agents operate within a controlled environment that provides:
  - File system access
  - Chat and terminal history tracking
  - Configurable environment variables

#### Agent Lifecycle

1. **Creation**: Agents are defined with metadata specifying:
   - Name and version
   - Default model configurations
   - Welcome messages
   - Environment variables

2. **Execution**: When run, an agent:
   - Is instantiated in a temporary directory
   - Has access to its entire file structure
   - Can interact with the provided environment
   - Supports multiple runtime configurations (Python/TypeScript)

### Experiment Platform

The platform includes a distributed experiment management system with two primary components:

- **Server**: 
  - Centralized coordination point
  - Manages experiment deployment across hosts
  - Tracks supervisor and experiment statuses

- **Supervisors**: 
  - Deployed on individual hosts
  - Receive and execute experiments
  - Report status back to the central server

#### Experiment Flow

1. Server starts and registers available supervisors
2. Pending experiments are matched with available supervisors
3. Experiments are assigned and executed
4. Supervisors report completion and become available again

### Key Abstractions

- **Environment**: Represents the runtime context for an agent
- **Registry**: Centralized storage and versioning system for agents
- **Multiprocessing**: Supports concurrent agent execution
- **Language-Agnostic Runtime**: Supports both Python and TypeScript agents

### Extensibility

The architecture is designed to be highly extensible, allowing:
- Custom agent implementations
- Flexible model and provider configurations
- Distributed computing across multiple hosts
- Seamless integration of different programming languages

### Runtime Considerations

- Agents run in isolated temporary directories
- Environment variables can be dynamically configured
- Supports user-specific runtime contexts
- Provides robust error handling and module management

## Extension Points

The framework provides robust extensibility through several key mechanisms, allowing developers to customize and extend its functionality.

### Tool Registration and Customization

Developers can extend the framework's capabilities by registering custom tools that agents can dynamically invoke. There are two primary methods for tool registration:

#### 1. Regular Python Function Tools
Create a Python function with a descriptive docstring and register it using the tool registry:

```python
def my_custom_tool(param1: str, param2: int):
    """
    A custom tool that performs a specific task.
    
    param1: Description of the first parameter
    param2: Description of the second parameter
    """
    # Implementation of the tool
    return result

# Register the tool
tool_registry = env.get_tool_registry()
tool_registry.register_tool(my_custom_tool)
```

#### 2. MCP (Model-Callable Procedure) Tools
For more complex tools with structured input schemas, use the MCP tool registration:

```python
from nearai.agents.models.tool_definition import MCPTool

# Define a tool with a specific input schema
mcp_tool = MCPTool(
    name="custom_weather_tool",
    description="Get detailed weather information for a location",
    inputSchema={
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "City and state, e.g., San Francisco, CA"
            },
            "details": {
                "type": "boolean",
                "description": "Whether to return extended weather details"
            }
        },
        "required": ["location"]
    }
)

async def call_custom_weather_api(name: str, args: dict):
    # Implement the actual API call or tool logic
    return weather_data

# Register the MCP tool
tool_registry.register_mcp_tool(mcp_tool, call_custom_weather_api)
```

### Agent Middleware and Customization

Agents can be extended through various configuration options:
- Custom system prompts
- Specific model configurations
- Tool integrations
- Environment variable customization

### Service Agent Integration

Developers can create specialized service agents that can be called by other agents to perform specific tasks, enabling complex workflow compositions.

### Extension Use Cases

1. **Custom Data Processing**: Create tools for specialized data transformations or analysis
2. **External API Integration**: Build tools that interact with third-party services
3. **Domain-Specific Workflows**: Develop agents with custom tools for specific industries or use cases
4. **Dynamic Tool Selection**: Allow agents to choose from a rich set of custom tools based on task requirements

### Best Practices

- Provide clear, descriptive docstrings for tools
- Design tools with well-defined input schemas
- Handle potential errors gracefully in tool implementations
- Use type hints to improve tool definition clarity

## Best Practices

### Code Quality and Maintainability

#### Project Structure
- Organize your project with clear, modular components
- Separate concerns by creating distinct modules for different functionalities
- Use meaningful and descriptive names for files, classes, and functions
- Keep your project structure consistent with the existing repository layout

#### Code Style and Formatting
- Follow the project's established code formatting guidelines
- Use `ruff` for code formatting and linting
- Run type checks with `mypy` to ensure type safety
- Write clear, concise comments that explain complex logic or non-obvious implementations

#### Development Workflow
- Create feature branches for new development
- Avoid working directly on the `main` branch
- Write descriptive commit messages that explain the purpose of your changes
- Before submitting a pull request, ensure all checks pass:
  ```bash
  ./scripts/format_check.sh
  ./scripts/lint_check.sh
  ./scripts/type_check.sh
  ```

#### Error Handling and Logging
- Implement robust error handling
- Use appropriate exception types
- Log errors and important events with meaningful context
- Provide informative error messages to aid debugging

#### Testing and Quality Assurance
- Write comprehensive unit tests for new functionality
- Aim for high test coverage
- Test edge cases and potential failure scenarios
- Use consistent testing methodologies across the project

#### Documentation
- Keep documentation up-to-date with code changes
- Document public APIs, functions, and classes
- Include examples and usage instructions
- Explain the purpose and behavior of complex components

#### Performance and Scalability
- Write efficient code with consideration for computational resources
- Use appropriate data structures and algorithms
- Profile and optimize performance-critical sections
- Consider scalability when designing new features or systems

#### Security Considerations
- Protect sensitive information (avoid hardcoding credentials)
- Use environment variables for configuration
- Implement proper access controls
- Follow secure coding practices
- Regularly update dependencies to patch known vulnerabilities

#### Dependency Management
- Use `pyproject.toml` for dependency specification
- Be mindful of dependency versions and potential conflicts
- Use virtual environments to isolate project dependencies
- Consider performance and compatibility when adding new dependencies

#### Collaboration and Code Review
- Be open to feedback during code reviews
- Provide clear, constructive feedback on pull requests
- Break large changes into smaller, manageable commits
- Communicate the motivation and context of your changes

## Project Structure

The project follows a modular and organized directory structure designed to separate concerns and support different components of the system:

#### Top-Level Directories

- `nearai/`: Core Python package containing the main application logic
  - `agents/`: Agent-related implementations and utilities
  - `aws_runner/`: AWS-specific runner implementations
  - `clients/`: Client interfaces and implementations
  - `finetune/`: Fine-tuning related modules
  - `openapi_client/`: Generated OpenAPI client code
  - `projects/`: Project-specific scripts and implementations
  - `shared/`: Shared utility functions and common components
  - `solvers/`: Problem-solving and benchmark-related modules

- `hub/`: Web application and backend services
  - `api/`: API route and endpoint implementations
  - `alembic/`: Database migration scripts
  - `demo/`: Frontend React/Next.js application
  - `examples/`: Code examples and demonstration scripts
  - `tasks/`: Background task implementations

- `ts_runner/`: TypeScript-based agent runner
  - `ts_agent/`: TypeScript agent implementations
  - `ts_agent_runner/`: TypeScript agent runtime and SDK

- `aws_runner/`: AWS-specific runner configurations and scripts
- `docs/`: Project documentation and markdown files
- `scripts/`: Utility scripts for building, testing, and CI/CD
- `worker/`: Worker-related Python modules

#### Key Configuration and Infrastructure Files

- `.docker/`: Dockerfiles for various runtime environments
- `.github/workflows/`: GitHub Actions CI/CD configuration
- `pyproject.toml`: Python project configuration
- `docker-compose.yaml`: Docker Compose configuration for local development

The structure emphasizes modular design, separating concerns between Python backend, TypeScript runtime, web frontend, and supporting infrastructure components.

## Technologies Used

### Programming Languages
- Python (Backend)
- TypeScript (Frontend and Tooling)

### Backend Frameworks and Libraries
- FastAPI: Web framework for building APIs
- LiteLLM: AI model interaction and abstraction
- SQLModel: ORM for database interactions
- Pydantic: Data validation and settings management
- Alembic: Database migration tool
- Transformers: Machine learning model handling
- Typer: CLI application development

### Frontend Technologies
- Next.js: React framework for web applications
- React: Frontend UI library
- React Query: Data fetching and state management
- tRPC: Typesafe API development
- Zustand: State management
- Zod: TypeScript-first schema validation

### AI and Machine Learning
- OpenAI API: AI model integration
- Hugging Face Transformers: Machine learning model handling
- VLLM: Large language model serving
- PyTorch: Machine learning framework

### Database and Storage
- SQLAlchemy: SQL database toolkit
- MySQL/PyMySQL: Database connectivity
- Boto3: AWS SDK for Python (S3 and other services)

### DevOps and Infrastructure
- Docker: Containerization
- Docker Compose: Multi-container Docker applications
- GitHub Actions: CI/CD workflows

### Testing and Development
- Pytest: Testing framework
- Mypy: Static type checking
- Ruff: Python linting and formatting
- ESLint: TypeScript/JavaScript linting
- Prettier: Code formatting

### Deployment and Hosting
- Uvicorn: ASGI server
- Gunicorn: Python WSGI HTTP Server
- AWS Lambda: Serverless compute

### Utilities and Tools
- Fire: Command-line interface generation
- Loguru: Logging
- Rich: Rich text and beautiful formatting
- Requests: HTTP library
- TQDM: Progress bars
- Python-dotenv: Environment variable management

### Blockchain
- Near.js: Near blockchain interactions
- Near Wallet Selector: Blockchain wallet integration

### Monitoring and Observability
- DataDog Tracing (ddtrace): Performance monitoring

## Additional Notes

### Data Storage and Configuration

NEAR AI stores all user-generated data and configuration files in the `~/.nearai` directory. This centralized location ensures easy management and persistence of your AI agents, experiments, and settings.

### Performance and Scalability Considerations

#### Distributed Architecture
The NEAR AI system is designed with a distributed architecture, featuring:
- A central hub for model serving and agent management
- Confidential execution environments
- Distributed job execution and scheduling
- Support for multiple runtime environments (AWS Lambda, local runners)

#### Agent Execution Environments
Agents can be run in multiple contexts:
- Local development using the CLI
- AWS Lambda via the AWS Runner
- Trusted Execution Environment (TEE) for enhanced security
- Containerized environments using Docker

### Security and Isolation

NEAR AI emphasizes agent security through:
- Environment isolation for agents
- Secure execution contexts
- Confidential computing support
- Built-in tools for managing agent permissions and access

### Experimental Features

The platform supports advanced experimental capabilities:
- Model fine-tuning for custom AI models
- Distributed task execution
- Agent-to-agent interactions
- Support for various machine learning frameworks

### Platform Compatibility

Supported Platforms:
- Python 3.11 (recommended)
- Linux, macOS, and Windows environments
- Docker-compatible systems
- NEAR blockchain ecosystem integration

### Monitoring and Observability

While developing and deploying agents, leverage:
- Built-in logging mechanisms
- Performance tracking
- Experiment result management
- Integration with popular monitoring tools

### Rate Limits and Usage

Be mindful of potential rate limits when:
- Deploying agents to the NEAR AI Hub
- Executing distributed tasks
- Interacting with external APIs and services

### Continuous Evolution

NEAR AI is an actively developed platform. Stay updated by:
- Following the project's GitHub repository
- Joining the Telegram community
- Checking the documentation for the latest features and improvements

## Contributing

We welcome contributions from the community! Whether you're fixing bugs, adding features, improving documentation, or helping others, your input is valuable.

### Ways to Contribute

#### Reporting Issues
- Check existing issues before creating a new one
- For bug reports, include:
  - Detailed description of the issue
  - Steps to reproduce
  - Expected vs. actual behavior
  - Environment details (OS, Python, PyTorch versions)
  - Full error traceback

#### Suggesting Features
- Open an issue describing:
  - Motivation for the feature
  - Detailed feature description
  - Potential usage example
  - Any related research or context

#### Code Contributions

##### Getting Started
1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/&lt;your-username&gt;/nearai.git
   cd nearai
   git remote add upstream https://github.com/nearai/nearai.git
   ```

3. Create a new branch for your changes:
   ```bash
   git checkout -b descriptive-branch-name
   ```

##### Development Guidelines
- Do not work directly on the `main` branch
- Run code quality checks before submitting:
  ```bash
  ./scripts/format_check.sh
  ./scripts/lint_check.sh
  ./scripts/type_check.sh
  ```
- Write clear, descriptive commit messages
- Keep pull requests focused and concise

##### Pull Request Process
- Provide a clear title summarizing your contribution
- Link any related issues
- Prefix work-in-progress PRs with `[WIP]`
- Ensure all checklist items are completed

### Code of Conduct
- Be respectful and inclusive
- Provide constructive feedback
- Help create a welcoming environment for all contributors

### Getting Help
If you need assistance:
- Check existing documentation
- Open an issue for questions
- Join our community discussions

## License

This project is licensed under the MIT License. 

### Terms

The MIT License is a permissive open-source license that allows you to:
- Use the software commercially
- Modify the software
- Distribute the software
- Use the software privately
- Use the software for commercial purposes

### Conditions

- Include the original copyright notice
- Include the original license in any substantial portion of the software

### Limitations

The license provides the software "as is" without warranty, and the authors are not liable for any claims or damages arising from its use.

For the full license text, see the [LICENSE](LICENSE) file in the repository.
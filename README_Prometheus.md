# NEAR AI: Open-Source Distributed AI Agent Platform for User-Owned AGI

## Project Overview

NEAR AI is an innovative open-source distributed system designed to democratize artificial intelligence by enabling the creation, deployment, and management of AI agents. The primary goal is to develop user-owned, open-source Artificial General Intelligence (AGI) through a flexible and extensible platform.

### Core Mission

The project aims to solve key challenges in AI development by providing:
- A decentralized infrastructure for building and running AI agents
- User-owned and controlled AI capabilities
- Seamless integration of AI agents across different environments

### Key Features

#### 1. Distributed Agent System
- Create, deploy, and manage AI agents with built-in tools
- Environment isolation for secure agent execution
- Support for multiple runtime environments (AWS Lambda, TEE)

#### 2. Comprehensive Agent Development
- Integrated CLI for agent creation and management
- Local and remote agent running capabilities
- Flexible agent deployment to NEAR AI Developer Hub

#### 3. Advanced Infrastructure
- Centralized hub for model serving and agent registry
- Distributed job execution and scheduling
- Confidential execution environments
- Model fine-tuning support

#### 4. Open and Collaborative
- Open-source architecture
- Community-driven development
- Transparent and user-controlled AI ecosystem

### Technical Components
- **NEAR AI Hub**: Central management system for agents and models
- **TEE Runner**: Confidential execution environment
- **AWS Runner**: Serverless agent execution
- **Agent System**: Core agent creation and management framework
- **Worker System**: Distributed computation infrastructure

The platform is designed to be flexible, scalable, and accessible, empowering developers and researchers to build advanced AI agents while maintaining user ownership and control.

## Getting Started, Installation, and Setup

### Prerequisites

Before getting started, ensure you have the following installed:

- [Python 3.11](https://www.python.org/downloads/) _(3.12+ currently not supported)_
- [Git](https://github.com/git-guides/install-git)
- [Docker](https://docs.docker.com/get-docker/) (optional, for local agent testing)

### Quick Installation

You can install NEAR AI using pip:

```bash
python3 -m pip install nearai
```

Or clone the repository for local development:

```bash
git clone https://github.com/nearai/nearai.git
cd nearai
./install.sh  # Install dependencies
# Or
python3 -m pip install -e .
```

### Authentication

Login to NEAR AI using your NEAR account. If you don't have an account, create one with [Meteor Wallet](https://wallet.meteorwallet.app):

```bash
nearai login
```

### Creating Your First Agent

Create a new AI agent with a simple command:

```bash
nearai agent create
```

You'll be prompted to provide:
- Agent name
- Short description
- Initial instructions

### Running Agents

Run your agent locally in interactive mode:

```bash
# Run a specific agent
nearai agent interactive <path-to-agent> --local

# Or select from your agents
nearai agent interactive --local
```

### Deploying Agents

Upload your agent to the NEAR AI Developer Hub:

```bash
nearai registry upload <path-to-agent>
```

### Updating NEAR AI

To update to the latest version:

```bash
cd nearai
git pull
python3 -m pip install -e .  # Refresh dependencies
```

### Next Steps

- Explore the [Official Documentation](https://docs.near.ai)
- Check out [Agent Development Guide](https://docs.near.ai/agents/quickstart)
- Join the [NEAR AI Community](https://t.me/nearaialpha)

## Core Concepts

## Key Architecture Components

### Agent Framework
The core of the system is a flexible, multi-language agent framework that supports both Python and TypeScript agents. Key components include:

#### Agent Lifecycle
1. **Agent Initialization**: 
   - Agents are created with metadata containing configuration details
   - Supports dynamic loading from a registry or local filesystem
   - Configurable with environment variables, model settings, and welcome messages

2. **Execution Environment**:
   - Each agent receives a dynamic environment object with advanced capabilities
   - Environment provides access to:
     * Conversation message history
     * User input requests
     * File storage and management
     * Inter-agent communication
     * Environment variables
     * Authentication context

3. **Runtime Support**:
   - Supports both Python and TypeScript agents
   - Handles code compilation and execution in isolated environments
   - Provides module import management and caching mechanisms
   - Supports multi-process execution for enhanced isolation

### System Architecture

#### Experiment Platform Components
The platform consists of two primary components:

1. **Server**:
   - Centralized deployment on a single node
   - Manages host coordination
   - Tracks experiment status and supervisor availability

2. **Supervisors**:
   - Deployed on individual hosts
   - Report availability status
   - Execute experiments and report results back to the server

#### Deployment Flow
1. Install `nearai` package
2. Configure host files
3. Install supervisors on all hosts
4. Start the server
5. Supervisors set their availability status
6. Server assigns experiments to available supervisors

### Key Features
- Decentralized experiment management
- Multi-language agent support
- Flexible environment configuration
- Secure isolated execution
- Dynamic module and file handling
- Configurable model and runtime parameters

### Execution Model
1. **Agent Loading**: Retrieve agent from registry or local filesystem
2. **Environment Preparation**: Set up runtime environment with variables
3. **Code Execution**: 
   - Isolate agent code in a temporary directory
   - Manage imports and module caching
   - Execute in a separate process for security
4. **Result Handling**: Capture and process agent output or errors

### Language Support
- **Python Agents**: Native Python code execution
- **TypeScript Agents**: Transpiled and run using Node.js
- Consistent interface across language implementations

## Extension Points

The framework provides flexible extension mechanisms to customize and expand agent functionality:

### Tool Registration and Extension

Developers can extend the agent's capabilities by registering new tools using the `ToolRegistry` system. Tools are functions that can be dynamically added to agents and called during execution.

#### How to Create and Register a Tool

1. Define a function with a clear name, docstring, and type hints:

```python
def get_current_weather(location: str, unit: Literal["celsius", "fahrenheit"] = "celsius") -> str:
    """Get the current weather in a given location.
    
    Args:
        location: The city and state, e.g. San Francisco, CA
        unit: Temperature unit (celsius or fahrenheit)
    
    Returns:
        A string describing the current weather
    """
    # Implementation details
```

2. Register the tool with the `ToolRegistry`:

```python
from nearai.agents.tool_registry import ToolRegistry

tool_registry = ToolRegistry()
tool_registry.register_tool(get_current_weather)
```

#### Tool Definition Requirements

- Function must have a descriptive docstring
- Use type hints for parameters
- Return a value that can be serialized

### Middleware and Custom Components

While the framework doesn't expose a formalized middleware system in the current implementation, agents can be customized through:

- Environment variables
- Custom code within agent implementations
- Tool extensions

### Common Extension Use Cases

1. **External API Integration**
   - Create tools that interact with third-party services
   - Example: A tool to fetch stock prices, retrieve data from external databases

2. **Domain-Specific Functionality**
   - Implement specialized tools for specific workflows
   - Example: A medical research agent with tools for literature search, data analysis

3. **Data Transformation and Processing**
   - Build tools that preprocess or transform data
   - Example: Text cleaning, format conversion, data validation tools

### Best Practices for Tool Development

- Keep tools focused and modular
- Handle potential errors gracefully
- Use type hints and docstrings for clarity
- Consider performance and external service limitations

### Advanced Tool Features

The `ToolRegistry` supports advanced tool registration, including:
- Automatic JSON schema generation
- Support for literal type constraints
- Dynamic parameter parsing and validation

### Limitations and Considerations

- Tools are statically registered
- Complex tools might require careful design to ensure reliability
- Performance can vary based on tool implementation

### Example of a Complex Tool

```python
def process_document(
    document_path: str, 
    output_format: Literal["json", "csv", "txt"] = "json", 
    process_mode: Literal["full", "summary"] = "summary"
) -> Dict[str, Any]:
    """Process a document with configurable output and processing modes.
    
    Args:
        document_path: Path to the document to process
        output_format: Desired output format
        process_mode: Level of processing detail
    
    Returns:
        Processed document data
    """
    # Implementation with multiple processing strategies
```

Developers are encouraged to explore and experiment with tool creation to extend the agent's capabilities.

## Best Practices

### Project Architecture and Code Organization

#### Code Structure
- Follow a modular approach, separating concerns across different modules and packages
- Use meaningful and descriptive names for classes, functions, and variables
- Keep functions and methods focused on a single responsibility
- Design components with high cohesion and low coupling

#### Agent and Tool Development
- When creating agents and tools, leverage the existing agent framework in `nearai/agents/agent.py`
- Use type hints and docstrings to clearly document function signatures and behaviors
- Implement robust error handling and provide meaningful error messages
- Design tools to be idempotent and stateless when possible

#### Dependency and Environment Management
- Use `pyproject.toml` for managing project dependencies
- Specify exact version constraints for critical dependencies
- Utilize virtual environments to isolate project dependencies
- Include a `.env.example` file to document required environment variables

#### Performance and Scalability
- Implement caching mechanisms for expensive computations (refer to `nearai/shared/cache.py`)
- Use asynchronous programming techniques for I/O-bound tasks
- Design agents and tools to be stateless and easily parallelizable
- Implement graceful degradation for scenarios with limited computational resources

#### Security Practices
- Never hardcode sensitive information like API keys or credentials
- Use the secure configuration management from `nearai/shared/client_config.py`
- Sanitize and validate all input data before processing
- Follow the principle of least privilege when designing agent permissions

#### Testing and Quality Assurance
- Write comprehensive unit tests for all components
- Use type checking with `mypy` to catch potential type-related issues
- Run linting with `ruff` to maintain consistent code quality
- Include integration tests that verify interactions between components

#### Documentation
- Maintain clear and concise docstrings for all public methods and classes
- Include type annotations for improved code readability
- Document any non-obvious implementation details or design decisions
- Keep documentation updated alongside code changes

#### Logging and Monitoring
- Use the logging utility in `nearai/log.py` for consistent logging
- Log errors, warnings, and important state changes
- Include contextual information in log messages for easier debugging
- Implement structured logging for better log analysis

#### Version Control and Collaboration
- Follow semantic versioning principles
- Write clear and descriptive commit messages
- Use feature branches and pull requests for collaborative development
- Include comprehensive PR descriptions explaining the motivation and implementation details

#### Framework and Technology Conventions
- Follow the conventions established in the project's core libraries
- Prefer composition over inheritance
- Use type hints and follow Python's type checking best practices
- Leverage existing utility functions in `nearai/lib.py` and similar modules

## Project Structure

The project is a complex, multi-component AI and agent development platform with several key directories:

### Main Project Directories

#### `nearai/`
The core Python package containing the main implementation of the AI framework:
- `agents/`: Core agent-related functionality
- `aws_runner/`: AWS-specific runner implementations
- `clients/`: Client implementations
- `finetune/`: Model fine-tuning utilities
- `openapi_client/`: OpenAPI client generation
- `projects/`: Example and specialized project implementations
- `shared/`: Shared utilities and helper functions
- `solvers/`: Problem-solving agents and implementations

#### `hub/`
The web application and backend services:
- `api/`: API route implementations
- `alembic/`: Database migration scripts
- `demo/`: Frontend web application (Next.js)
- `examples/`: Example scripts and code samples
- `tasks/`: Background task implementations

#### `ts_runner/`
TypeScript-specific runner and agent implementations:
- `ts_agent/`: TypeScript agent core
- `ts_agent_runner/`: Agent runner SDK

#### `aws_runner/`
AWS-specific deployment and runner configurations:
- `frameworks/`: Requirements and framework-specific configurations
- `local_runners/`: Local development runner configurations
- `py_runner/`: Python runtime runner
- `ts_runner/`: TypeScript runtime runner

### Supporting Directories

#### `docs/`
Comprehensive documentation:
- `agents/`: Agent-specific documentation
- `examples/`: Code and tutorial examples
- `tutorials/`: In-depth guides and learning resources

#### `scripts/`
Development and build utility scripts:
- Build tools
- Testing scripts
- Code generation utilities

#### `etc/`
Configuration and setup files:
- Environment configurations
- Service definitions
- Installation scripts

### Key Configuration Files
- `pyproject.toml`: Python project configuration
- `docker-compose.yaml`: Docker composition setup
- Various Dockerfiles for different runtime environments

### Development and CI/CD
- `.github/workflows/`: GitHub Actions for continuous integration
- Multiple environment-specific Dockerfiles
- Comprehensive testing and linting configurations

The project is designed with a modular, extensible architecture supporting both Python and TypeScript implementations, with robust support for AI agent development, deployment, and management.

## Technologies Used

### Programming Languages
- Python (primary backend language)
- TypeScript (frontend and secondary runtime)
- Bash (scripting)

### Backend Frameworks and Libraries
- FastAPI: Web framework for building APIs
- Pydantic: Data validation and settings management
- SQLModel: ORM for database interactions
- Alembic: Database migration tool
- LiteLLM: Unified interface for LLM integrations
- Transformers: Machine learning model handling
- OpenAI Python Library: AI model interactions
- Fireworks AI: AI model integration

### Frontend Technologies
- Next.js: React framework for web applications
- React: JavaScript library for UI
- tRPC: Typesafe API development
- React Query: State management and data fetching
- Zustand: State management
- Zod: TypeScript-first schema validation

### Machine Learning and AI
- PyTorch: Deep learning framework
- Hugging Face Transformers: Pre-trained model library
- vLLM: Large language model serving
- PEFT: Parameter-Efficient Fine-Tuning

### Database and Storage
- SQLAlchemy: SQL toolkit and ORM
- PyMySQL: MySQL database connector
- Boto3: AWS SDK for Python

### DevOps and Infrastructure
- Docker: Containerization
- Docker Compose: Multi-container Docker applications
- GitHub Actions: CI/CD workflows
- Uvicorn: ASGI server

### Development and Testing Tools
- Pytest: Testing framework
- Ruff: Python linter and formatter
- MyPy: Static type checker
- Commitizen: Conventional commit messages
- ESLint: TypeScript/JavaScript linter

### Blockchain and Web3
- Near Wallet Selector: Multi-wallet support
- Near.js: Near blockchain interaction
- py-near: Python library for Near protocol

### Utilities and Helpers
- Typer: CLI application development
- Rich: Rich text and beautiful formatting
- Loguru: Logging library
- Tenacity: Retry decorator
- TQDM: Progress bar

### Documentation
- MkDocs: Documentation generation
- MkDocs Material: Documentation theme

### Optional/Extended Capabilities
- Flask: Lightweight web framework
- Torch AO: PyTorch acceleration
- Lean Dojo: Theorem proving environment

## Additional Notes

### Data Storage and Persistence

All data created by `nearai` is stored in the `~/.nearai` directory. This includes:
- Local configuration settings
- Cached data
- Experiment logs
- User-specific information

### Experiment Platform Architecture

NEAR AI features a distributed experiment platform with two primary components:
- **Server**: Centralized node with SSH access to all hosts
- **Supervisors**: Deployed on each individual host

#### Key Deployment Considerations
- The platform is designed for scalable, distributed task execution
- Supervisors can dynamically set their availability status
- Experiments are assigned to available supervisors
- The system ensures efficient resource utilization and task distribution

### Performance and Scalability

- Support for running agents across multiple hosts
- Dynamic host management through supervisor registration
- Idempotent deployment process for easy setup and maintenance

### Security and Confidentiality

- Built-in support for Trusted Execution Environments (TEE)
- Confidential execution environments for AI agents and inference
- Secure agent environment isolation

### Development and Extensibility

The platform is designed with modularity in mind:
- Flexible agent creation and deployment
- Multiple runtime environments (AWS Lambda, local runners)
- Support for various AI model fine-tuning scenarios

### Platform Limitations and Considerations

- Currently supports Python 3.11 (Python 3.12+ not supported)
- Requires Docker for local agent testing
- Dependent on NEAR blockchain ecosystem for authentication and infrastructure

## Contributing

We welcome contributions from the community! Whether you're fixing bugs, adding features, improving documentation, or helping others, your contributions are valuable.

### How to Contribute

#### Ways to Contribute
- Report bugs or issues
- Suggest new features
- Improve documentation
- Submit pull requests
- Help answer questions

#### Getting Started
1. **Fork the Repository**: Create a fork of the `nearai` repository on GitHub.

2. **Set Up Development Environment**:
   ```bash
   git clone git@github.com:<your Github handle>/nearai.git
   cd nearai
   git remote add upstream https://github.com/nearai/nearai.git
   ```

3. **Create a Branch**:
   ```bash
   git checkout -b a-descriptive-name-for-my-changes
   ```

#### Code Contribution Guidelines
- Do not work directly on the `main` branch
- Write clear, descriptive commit messages
- Follow the project's code style and formatting

#### Code Quality Checks
Before submitting a pull request, run the following checks:
```bash
./scripts/format_check.sh
./scripts/lint_check.sh
./scripts/type_check.sh
```

#### Submitting a Pull Request
- Ensure the pull request title clearly describes your contribution
- Link any related issues in the pull request description
- Prefix work-in-progress pull requests with `[WIP]`

#### Reporting Issues
When reporting a bug, please include:
- Steps to reproduce the issue
- Expected vs. actual behavior
- Your OS, Python, and library versions
- A minimal code snippet demonstrating the problem
- Full traceback of any exceptions

#### Documentation Contributions
To preview documentation changes locally:
```bash
pip install -e ".[docs]"  # or uv sync --group docs
mkdocs serve
```

Thank you for helping improve `nearai`!

## License

This project is licensed under the MIT License. 

### Key Permissions
- Commercial use
- Modification
- Distribution
- Private use

### Conditions
- License and copyright notice must be included
- No warranty or liability

For full details, please see the [LICENSE](LICENSE) file in the repository.

#### License Details
- **Type**: MIT License
- **Copyright**: © 2025 NEAR AI
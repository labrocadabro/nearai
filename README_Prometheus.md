# NEAR AI: Democratizing Distributed AI Agent Development

## Project Overview

NEAR AI is a comprehensive distributed system designed to revolutionize AI agent development, deployment, and management. The platform aims to create an open-source, user-owned Artificial General Intelligence (AGI) ecosystem that democratizes AI technology.

### Core Purpose

The primary goal of NEAR AI is to provide developers and researchers with a flexible, scalable infrastructure for building, deploying, and running AI agents across distributed environments. It addresses key challenges in AI development by offering:

- A centralized hub for model serving and agent registry
- Secure and isolated execution environments
- Tools for agent creation and management
- Distributed job execution and scheduling
- Support for model fine-tuning

### Key Features

- **Distributed Agent Execution**: Run AI agents across multiple environments including AWS Lambda, confidential TEE (Trusted Execution Environment), and local systems
- **Agent System**: Build AI agents with built-in tools and comprehensive environment isolation
- **Flexible Deployment**: Easy deployment to the NEAR AI Developer Hub with simple CLI commands
- **Open Source Ecosystem**: Community-driven platform encouraging collaboration and innovation
- **Multi-Environment Support**: Compatible with various computational backends and frameworks
- **Secure Infrastructure**: Emphasis on confidential computing and user-owned AI technologies

### Benefits

- Simplifies AI agent development with a comprehensive toolkit
- Provides a decentralized approach to AI agent creation and management
- Enables rapid prototyping and deployment of intelligent agents
- Supports collaborative AI research and development
- Reduces infrastructure complexity for AI projects
- Promotes transparency and user ownership in AI technologies

## Getting Started, Installation, and Setup

### Prerequisites

Before getting started, ensure you have the following requirements:

- [Python 3.9-3.11](https://www.python.org/downloads/) _(3.12+ currently not supported)_
- [Git](https://github.com/git-guides/install-git)
- [Docker](https://docs.docker.com/get-docker/) (optional, for local agent testing)

### Installation

There are multiple ways to install NEAR AI:

#### Option 1: Install via pip

```bash
python3 -m pip install nearai
```

Verify the installation:
```bash
nearai version
```

#### Option 2: Install from Source

Clone the repository and install:
```bash
git clone https://github.com/nearai/nearai.git
cd nearai
./install.sh  # Recommended method
```

Alternatively, install directly with pip:
```bash
python3 -m pip install -e .
```

### Quick Start Guide

#### 1. Login to NEAR AI

Create a free account with [Meteor Wallet](https://wallet.meteorwallet.app) and log in:

```bash
nearai login
```

#### 2. Create an Agent

Create a new AI agent interactively:

```bash
nearai agent create
```

Follow the prompts to:
- Name your agent
- Add a description
- Set initial instructions

#### 3. Run Your Agent

Run the agent locally in interactive mode:

```bash
# Run a specific agent
nearai agent interactive <path-to-agent> --local

# Or select from your created agents
nearai agent interactive --local
```

#### 4. Deploy Your Agent (Optional)

Upload your agent to the NEAR AI public registry:

```bash
nearai registry upload <path-to-agent>
```

### Development vs. Production

#### Local Development
- Use `nearai agent interactive --local` for testing
- Agents run in a local environment with predefined configurations

#### Production Deployment
- Use `nearai registry upload` to publish to the public registry
- Agents can be embedded in websites or used via the NEAR AI platform

### Updating NEAR AI

To update the NEAR AI CLI and dependencies:

```bash
cd nearai
git pull
python3 -m pip install -e .
```

### Additional Resources
- [Official Documentation](https://docs.near.ai)
- [Agent Development Guide](https://docs.near.ai/agents/quickstart)
- [Community Support](https://t.me/nearaialpha)

## Core Concepts

The project is built around a flexible agent-based architecture designed to enable intelligent, composable AI systems. Key concepts include:

### Agents

Agents are the primary building blocks of the system. Each agent is a programmable entity with the following core characteristics:

- **Composition**: Agents can be written in Python or TypeScript
- **Metadata**: Stored with a specific versioning structure: `namespace/name/version`
- **Execution Environment**: 
  - Agents run in an isolated, temporary directory
  - Supports dynamic file management and tool interactions
  - Can access system and thread-specific resources

### Environment

The `Environment` is a sophisticated execution context that provides:

- **Resource Management**: 
  - File system abstraction
  - Logging capabilities
  - Secure execution of commands and tools
- **Communication Mechanisms**:
  - Message handling
  - Tool registry and execution
  - Inter-agent communication
- **AI Interaction Capabilities**:
  - Model inference
  - Completions with optional tool calls
  - Signed message verification

### Tool Registry and Execution

The system includes a robust tool registration and execution framework:

- **Tool Definition**: Tools can be dynamically registered and called
- **Execution Strategies**:
  - Supports multiple tool invocation formats
  - Handles tool calls from different model providers
- **Built-in Tools**: Includes standard tools like file read/write, command execution, and vector store querying

### Inference and Model Interaction

The architecture provides flexible AI model interaction:

- **Model Abstraction**: Supports multiple providers and model types
- **Inference Modes**:
  - Synchronous and streaming completions
  - Tool-augmented generation
  - Signed completions
- **Dynamic Model Selection**: Can dynamically choose and configure model parameters

### Concurrency and Execution

The system supports advanced execution models:

- **Multi-agent Environments**: Can run multiple agents in a single context
- **Thread Management**: 
  - Supports thread forking
  - Enables complex conversation flows
- **Run Modes**: Flexible execution strategies (simple, advanced)

### Security and Isolation

Built-in security features include:

- **Execution Sandboxing**: Agents run in isolated temporary directories
- **Command Approval**: Configurable command execution approval
- **Signature Verification**: Support for cryptographically signed messages

### Extensibility

The architecture is designed to be highly extensible:

- **Pluggable Tool System**: Easy addition of new tools and capabilities
- **Multi-language Support**: Agents can be written in Python or TypeScript
- **Flexible Configuration**: Supports various runtime and environment configurations

## Extension Points

## Extension Points

The framework provides multiple robust mechanisms for developers to extend and customize functionality:

### Tool Registration

Extend the agent's capabilities by registering custom tools that can be invoked by the language model. There are two primary methods for tool registration:

#### 1. Standard Function Tools
Register regular Python functions as tools that can be dynamically called by the agent:

```python
def my_custom_tool(param1: str, param2: int):
    """
    A custom tool that performs a specific task.
    This docstring helps the LLM understand when and how to invoke the tool.
    """
    # Implementation of your tool
    return result

# Register the tool with the tool registry
tool_registry = env.get_tool_registry()
tool_registry.register_tool(my_custom_tool)
```

#### 2. MCP (Multi-Call Protocol) Tools
For more complex tools with advanced configuration, use the MCP tool registration:

```python
from nearai.agents.models.tool_definition import MCPTool

# Define a tool with a specific input schema
mcp_tool = MCPTool(
    name="weather_lookup",
    description="Get current weather for a location",
    inputSchema={
        "type": "object",
        "properties": {
            "location": {
                "type": "string",
                "description": "City and state"
            }
        },
        "required": ["location"]
    }
)

# Provide an async function to handle the tool call
async def weather_api_handler(name: str, args: dict):
    # Implement your API call or logic
    return weather_data
    
tool_registry.register_mcp_tool(mcp_tool, weather_api_handler)
```

### Use Cases for Extension

1. **Custom API Integrations**: Create tools that interact with external services
2. **Data Retrieval**: Implement functions to fetch information from various sources
3. **Complex Computation**: Add specialized computational tools for specific domains
4. **Environment Interaction**: Create tools for file manipulation, system commands, or database queries

### Deployment and Invocation

Once registered, tools can be used in agent interactions:

```python
# Get all tool definitions (built-in and custom)
all_tools = env.get_tool_registry().get_all_tool_definitions()

# Run completion with available tools
response = env.completions_and_run_tools(messages, tools=all_tools)
```

### Additional Extension Options

- **Language Support**: Supports both Python and TypeScript agent implementations
- **Flexible Environment Configuration**: Customize agent behavior through environment variables
- **Event-Driven Architecture**: Agents can be triggered and can interact with events

### Best Practices

- Provide clear, descriptive docstrings for tools
- Define precise input schemas for complex tools
- Handle potential errors and edge cases in tool implementations
- Consider performance and side effects of tool executions

## Best Practices

### Code Organization and Structure

#### Project Architecture
- Follow a modular approach when designing agents and components
- Separate concerns by creating discrete modules for different functionalities
- Utilize the environment object for managing agent interactions and state

#### Coding Conventions
- Adhere to Python type hints and use `mypy` for static type checking
- Use `ruff` for consistent code formatting and linting
- Write clear, descriptive commit messages that explain the purpose of changes
- Include comprehensive docstrings for functions, classes, and modules

#### Agent Development Best Practices
- Design agents with single responsibility principles
- Use environment methods for context-aware interactions
- Implement robust error handling and logging
- Leverage tools and tool registries for extensible functionality

#### Performance and Scalability
- Optimize agent performance by minimizing unnecessary computations
- Use caching mechanisms for repeated or expensive operations
- Design agents to be stateless when possible to improve scalability
- Utilize asynchronous programming for I/O-bound tasks

#### Security Considerations
- Never hardcode sensitive information like API keys or credentials
- Use the built-in secrets management for handling confidential data
- Implement proper authentication and authorization checks
- Validate and sanitize all user inputs and external data sources

#### Testing and Reliability
- Write comprehensive unit tests for each agent and utility function
- Use integration tests to verify component interactions
- Implement logging to capture important events and potential issues
- Design agents to be idempotent and resilient to unexpected inputs

#### Documentation
- Maintain clear and up-to-date documentation for all custom agents
- Include usage examples, input/output specifications, and potential limitations
- Document any external dependencies or special configuration requirements
- Use inline comments to explain complex logic or non-obvious implementations

#### Continuous Integration
- Set up CI/CD pipelines to automatically run:
  - Code formatting checks
  - Type checking
  - Linting
  - Unit and integration tests
- Regularly update dependencies and check for potential security vulnerabilities

#### Configuration Management
- Utilize environment variables for configurable parameters
- Provide sensible default configurations
- Allow easy overriding of default settings through configuration files or environment variables

## Project Structure

The project is organized into several key directories, each serving a specific purpose:

### Core Project Directories

#### `nearai/`
The main Python package containing the core implementation of the project:
- `agents/`: Core agent-related functionality
  - `agent.py`: Base agent implementation
  - `environment.py`: Agent environment configuration
  - `tool_registry.py`: Tool management and registration
- `cli.py`: Command-line interface implementation
- `openapi_client/`: OpenAPI client generated code for API interactions
- `shared/`: Shared utilities and helper functions

#### `hub/`
Web application and backend services:
- `api/`: API route implementations
- `alembic/`: Database migration scripts
- `demo/`: Frontend React application with Next.js
- `tasks/`: Background task implementations
- `examples/`: Code examples and usage demonstrations

#### `aws_runner/`
Cloud and runner-specific implementations:
- `frameworks/`: Requirements files for different frameworks
- `local_runners/`: Local runner configurations
- `py_runner/`: Python runtime configurations
- `ts_runner/`: TypeScript runtime configurations

#### `ts_runner/`
TypeScript-specific agent runner:
- `ts_agent/`: TypeScript agent implementation
- `ts_agent_runner/`: Agent runner SDK and utilities

#### `scripts/`
Utility scripts for:
- Build processes
- Code generation
- Testing
- Linting and formatting

### Supporting Directories

- `docs/`: Project documentation
- `etc/`: Configuration and system setup files
- `worker/`: Worker-related implementations
- `.github/`: GitHub workflow and issue template configurations

### Key Configuration Files

- `pyproject.toml`: Python project configuration
- `docker-compose.yaml`: Docker Compose configuration
- Multiple Dockerfiles in `.docker/` for different runtime environments

The project follows a modular architecture, separating concerns between agent implementation, web services, runners, and utility functions.

## Additional Notes

### Data Storage and Configuration

All data created by `nearai` is automatically stored in the `~/.nearai` directory. This includes configuration files, experiment data, and other persistent information. Users should be aware of this default storage location when managing their project data.

### Experimental Platform Architecture

The NEAR AI platform includes a distributed experimental system with two key components:
- **Server**: Centrally deployed with SSH access to all hosts
- **Supervisors**: Deployed on individual hosts for task management

#### Key Platform Behaviors
- Supervisors self-manage their availability status
- Experiments are dynamically assigned to available supervisors
- The system supports idempotent deployment and supervisor installation

### Environment and Runtime Considerations

#### Supported Python Versions
- **Recommended**: Python 3.11
- **Warning**: Python 3.12+ is currently not supported

#### Dependency Management
- Uses `uv` for environment synchronization
- Supports installation via pip, local setup, and virtual environment
- Recommends using the provided `install.sh` script for comprehensive setup

### Telemetry and Community Support

- Active Telegram community for developer support: [@nearaialpha](https://t.me/nearaialpha)
- Encourages community contributions through bug reports, feature suggestions, and documentation improvements

### Security and Authentication

- Requires authentication via NEAR Account
- Recommends [Meteor Wallet](https://wallet.meteorwallet.app) for account creation
- Supports login through CLI with `nearai login` command

### Debugging and Maintenance

#### Update Process
```bash
cd nearai
git pull
python3 -m pip install -e .  # Refresh dependencies if changed
```

### Monitoring and Logging

- Default logging and data storage in `~/.nearai`
- Supports comprehensive experiment tracking and task management through the distributed platform

## Contributing

We warmly welcome contributions from the community! Whether you're fixing bugs, improving documentation, or adding new features, your help is appreciated.

### Ways to Contribute

1. **Report Issues**
   - Check existing issues before creating a new one
   - Provide clear, detailed descriptions when reporting bugs or suggesting features
   - Include relevant context such as:
     * Your operating system
     * Python version
     * Specific steps to reproduce the issue
     * Full error traceback, if applicable

2. **Code Contributions**

#### Preparation
- Fork the repository
- Create a new branch for your changes (`git checkout -b feature/your-feature-name`)
- Set up a development environment using `uv sync`

#### Code Standards
We use the following tools to maintain code quality:
- [`ruff`](https://docs.astral.sh/ruff/) for linting
- [`mypy`](https://mypy.readthedocs.io/en/stable/) for type checking

Before submitting a pull request, run the following checks:
```bash
./scripts/format_check.sh    # Check code formatting
./scripts/lint_check.sh      # Run linter
./scripts/type_check.sh      # Run type checker
```

#### Pull Request Guidelines
- Use descriptive, concise titles
- Link to related issues if applicable
- Prefix work-in-progress PRs with `[WIP]`
- Ensure all automated checks pass
- Be open to feedback and requested changes

### Documentation Contributions
- If you find documentation errors or gaps, please open an issue or submit a pull request
- To preview documentation changes locally:
  ```bash
  pip install -e ".[docs]"  # Install documentation dependencies
  mkdocs serve              # Start local documentation server
  ```

### Code of Conduct
Be respectful, inclusive, and considerate of others. We aim to maintain a welcoming environment for all contributors.

### Questions?
If you have questions about contributing, feel free to open an issue for guidance.

## License

This project is licensed under the MIT License. 

### Full License Text

The complete license can be found in the [LICENSE](LICENSE) file in the repository root. 

### Key Permissions

- Commercial use
- Modification
- Distribution
- Private use

### Limitations

- No warranty
- No liability

### Conditions

- Include the original license and copyright notice in any substantial portion of the software

### Copyright

Copyright (c) 2025 NEAR AI

For the full license details and complete terms, please refer to the [LICENSE](LICENSE) file.
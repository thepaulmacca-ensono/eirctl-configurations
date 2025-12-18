# Eirctl Configuration Files

A curated collection of reusable task configurations for [eirctl](https://github.com/ensono/eirctl) - enabling streamlined CI/CD workflows across Azure DevOps and GitHub pipelines.

## What is this?

**eirctl-configs** provides pre-built, production-ready task definitions that can be imported directly into your `eirctl` task runner. These configurations cover common DevOps operations including:

- **Infrastructure as Code** - Terraform formatting, validation, planning, and deployment
- **Security Scanning** - Static code analysis with Checkov
- **Pipeline Management** - Azure DevOps pipeline retention policies
- **Git Operations** - Automated tagging for releases and builds
- **Code Quality** - YAML linting and Terraform documentation generation

## Features

- 🔧 **Ready-to-use tasks** - Drop-in configurations for common DevOps workflows
- 🐳 **Containerized execution** - Tasks run in the `ensono/eir-infrastructure` container for consistent environments
- 🔄 **Multi-platform support** - Configurations for both Azure DevOps and GitHub Actions
- 📦 **Modular design** - Import only what you need from organized YAML files
- 🛡️ **Security-focused** - Built-in scanning and linting tasks

## Getting Started

### Prerequisites

- [eirctl](https://github.com/ensono/eirctl) installed
- Docker (for containerized task execution)
- Azure CLI credentials (for Azure-related tasks)

### Installation

Reference the configuration files directly in your `eirctl` configuration by importing them from this repository:

```yaml
imports:
  - https://raw.githubusercontent.com/thepaulmacca-ensono/eirctl-configs/main/infra/terraform.yaml
  - https://raw.githubusercontent.com/thepaulmacca-ensono/eirctl-configs/main/security/scanning.yaml
  - https://raw.githubusercontent.com/thepaulmacca-ensono/eirctl-configs/main/shared/contexts.yaml
```

Or clone the repository and reference local files:

```bash
git clone https://github.com/thepaulmacca-ensono/eirctl-configs.git
```

### Usage

Once imported, you can run tasks using `eirctl`:

```bash
# Run Terraform format check
eirctl run lint:terraform:format

# Run Terraform plan
eirctl run terraform:plan

# Run security scanning with Checkov
eirctl run scan:terraform:checkov

# Tag a commit in Azure DevOps
eirctl run ado:git:tag
```

## Available Tasks

### Infrastructure (`infra/terraform.yaml`)

| Task | Description |
|------|-------------|
| `lint:terraform:format` | Check Terraform formatting |
| `lint:terraform:validate` | Validate Terraform configuration |
| `lint:terraform:tflint` | Run TFLint static analysis |
| `terraform:init` | Initialize Terraform with backend config |
| `terraform:plan` | Generate Terraform execution plan |
| `terraform:apply` | Apply Terraform plan |
| `terraform:destroy:plan` | Generate destroy plan |
| `terraform:destroy:apply` | Apply destroy plan |
| `terraform:ado:outputs` | Export outputs to ADO variable groups |
| `doc:terraform:generate` | Generate Terraform documentation |

### Security (`security/`)

| Task | Description |
|------|-------------|
| `lint:yaml` | YAML linting with yamllint |
| `scan:terraform:checkov` | Terraform security scanning with Checkov |

### Azure DevOps (`azure_devops/`)

| Task | Description |
|------|-------------|
| `ado:retain:pipeline` | Retain pipeline runs for a specified period |
| `ado:git:tag` | Tag a commit with service name and build number |
| `ado:git:release:tag` | Tag a commit for release |

### GitHub (`github/`)

| Task | Description |
|------|-------------|
| `github:git:tag` | Tag a commit with service name and run number |

## Configuration

### Environment Variables

Tasks use environment variables for configuration. Common variables include:

| Variable | Description |
|----------|-------------|
| `TF_FILE_LOCATION` | Path to Terraform files |
| `TF_BACKEND_INIT` | Terraform backend initialization arguments |
| `ENVIRONMENT_NAME` | Target environment/workspace name |
| `SERVICE_NAME` | Service identifier for tagging |
| `ADO_ACCESS_TOKEN` | Azure DevOps personal access token |
| `ADO_ORGANISATION` | Azure DevOps organization name |

### Contexts

The shared context (`shared/contexts.yaml`) defines execution environments:

```yaml
contexts:
  powershell:
    container:
      name: ensono/eir-infrastructure
      container_args:
        - -v ~/.azure-container:/root/.azure
      shell: pwsh
```

## Project Structure

```
eirctl-configs/
├── azure_devops/       # Azure DevOps specific tasks
│   ├── pipelines.yaml  # Pipeline management tasks
│   └── tagging.yaml    # Git tagging for ADO
├── github/             # GitHub specific tasks
│   └── tagging.yaml    # Git tagging for GitHub Actions
├── infra/              # Infrastructure tasks
│   └── terraform.yaml  # Terraform workflow tasks
├── security/           # Security and quality tasks
│   ├── linting.yaml    # Code linting tasks
│   └── scanning.yaml   # Security scanning tasks
└── shared/             # Shared configurations
    └── contexts.yaml   # Execution contexts
```

## Support

- 📖 [eirctl Documentation](https://github.com/ensono/eirctl)
- 🐛 [Report Issues](https://github.com/thepaulmacca-ensono/eirctl-configs/issues)
- 💬 [Discussions](https://github.com/thepaulmacca-ensono/eirctl-configs/discussions)

## Contributing

Contributions are welcome! Please read the contribution guidelines before submitting pull requests.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-task`)
3. Commit your changes (`git commit -am 'Add new task'`)
4. Push to the branch (`git push origin feature/new-task`)
5. Open a Pull Request

## License

See the [LICENSE](LICENSE) file for details.

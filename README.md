# GitHub Workflows

A collection of commonly-used GitHub Actions workflows for various automation tasks.

## Available Workflows

### 1. sync-mirror (`mirror.yml`)

Automatically mirrors a GitHub repository to Gitee.

**Triggers:**
- `push` - When code is pushed to the repository
- `delete` - When branches or tags are deleted
- `create` - When branches or tags are created

**Features:**
- Uses SSH authentication for secure mirroring
- Supports both GitHub and Gitee platforms
- Runs with concurrency control to prevent duplicate runs

**Required Secrets:**
- `SSH_PRIVATE_KEY` - SSH private key (Ed25519 format) for authentication. The corresponding public key should be added to both GitHub and Gitee accounts.

**Usage:**
1. Generate an SSH key pair on your local machine:
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```
2. Add the public key to your GitHub account (Settings → SSH and GPG keys)
3. Add the public key to your Gitee account (Settings → SSH Public Keys)
4. Add the private key as a repository secret named `SSH_PRIVATE_KEY`
5. Place `mirror.yml` in your repository's `.github/workflows/` directory

---

### 2. PR Agent (`pr_agent.yml`)

AI-powered pull request automation using the Qodo PR Agent for code review, description generation, and improvement suggestions.

**Triggers:**
- `pull_request` (opened, reopened, ready_for_review)
- `issue_comment`

**Features:**
- Automatic PR review with AI analysis
- Automatic PR description generation
- Automatic code improvement suggestions
- Skips actions triggered by bots
- Uses Zhipu AI's GLM-4.7 model

**Required Secrets:**
- `API_KEY` - API key for Zhipu AI (or other LLM provider). Get it from [Zhipu AI](https://open.bigmodel.cn/).

**Configuration:**
- Primary model: `glm-4.7`
- Fallback model: `glm-4.6`
- API base: `https://open.bigmodel.cn/api/paas/coding/v4/`
- Auto-enabled features: review, describe, improve

**Usage:**
1. Obtain an API key from Zhipu AI platform
2. Add the API key as a repository secret named `API_KEY`
3. Place `pr_agent.yml` in your repository's `.github/workflows/` directory
4. The agent will automatically analyze PRs when they are opened or updated

---

### 3. Test Python (`test_python.yml`)

Comprehensive Python testing workflow with GPU support and code coverage reporting.

**Triggers:**
- `push` (except to `gh_pages` branch)
- `pull_request`

**Features:**
- Python 3.11 environment setup
- Disk space optimization for CI runners
- DeepMD-kit installation with GPU and PyTorch support
- Automated testing with pytest
- Code coverage reporting to Codecov
- Test results reporting to Codecov

**Required Secrets:**
- `CODECOV_TOKEN` - Token generated in Codecov for your repository. Get it from [Codecov](https://codecov.io/).

**Dependencies:**
- Installs `deepmd-kit[gpu,cu12,torch]` from a custom fork
- Installs package with test and torch_admp extras

**Usage:**
1. Create an account on [Codecov](https://codecov.io/)
2. Add your repository to Codecov
3. Copy the repository token
4. Add the token as a repository secret named `CODECOV_TOKEN`
5. Place `test_python.yml` in your repository's `.github/workflows/` directory
6. Ensure your project has a `tests/` directory with pytest-compatible tests

---

## Required Secrets Summary

| Secret | Used By | Description |
|--------|---------|-------------|
| `SSH_PRIVATE_KEY` | sync-mirror | SSH private key for GitHub/Gitee authentication |
| `CODECOV_TOKEN` | Test Python | Token for uploading coverage reports to Codecov |
| `API_KEY` | PR Agent | API key for Zhipu AI (or other LLM provider) |

## Installation

To use any of these workflows in your repository:

1. Copy the desired `.yml` file to your repository's `.github/workflows/` directory
2. Configure the required secrets in your repository settings (Settings → Secrets and variables → Actions)
3. Commit and push the workflow file
4. The workflow will automatically trigger based on its defined events

## License

See [LICENSE](LICENSE) for details.

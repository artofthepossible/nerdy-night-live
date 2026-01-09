# 🐳 Container Comedy Central with Docker Sandboxes

> A secure, sandboxed AI comedian specializing in Docker and Kubernetes humor—powered by [cagent](https://github.com/docker/cagent) and [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/)

---

## 🎭 About

Container Comedy Central is your AI-powered stand-up comedian that turns DevOps pain into laughter. Built as a multi-agent system with cagent, this project demonstrates how to safely run AI agents using Docker Sandboxes—an experimental feature that isolates AI agents from your local machine while preserving a familiar development experience.

### Why Docker Sandboxes?

When AI agents can execute shell commands and modify files, security becomes critical. Docker Sandboxes provides:

- **Isolation**: Agents run in containers, not directly on your host machine
- **Path Consistency**: Your workspace appears at the same path in the container
- **State Persistence**: One sandbox per workspace preserves installed packages across sessions
- **Credential Management**: Automatic Git credential injection for proper attribution
- **Controlled Access**: Agents can only access your workspace directory, nothing else

---

## 🛠️ Tech Stack

- **Docker Sandboxes** (experimental security feature in Docker Desktop 4.50+)
- **cagent** (multi-agent runtime)
- **YAML** (agent configuration)
- **OpenAI / Anthropic** (AI model providers)

---

## 📋 Prerequisites

### Required
- **Docker Desktop 4.50 or later** with Sandboxes feature enabled
- **cagent binary** - Download from [GitHub releases](https://github.com/docker/cagent/releases)
- **API keys** for your preferred AI provider (OpenAI, Anthropic, etc.)


---

## 🚀 Quick Start with Docker Sandboxes

### 1. Enable Docker Sandboxes

First, ensure you have Docker Desktop 4.50+ installed:

```bash
docker --version
```

Docker Sandboxes is currently an experimental feature. Enable it in Docker Desktop:
1. Open Docker Desktop
2. Go to Settings → Features in development
3. Enable "Docker Sandboxes"

### 2. Download & Install cagent
```bash
Download the prebuilt binary for your [platform](https://github.com/docker/cagent/releases
```


Verify installation:
```bash
cagent --version
```

### 3. Clone This Repository

```bash
git clone <your-repo-url>
cd cagent-comedy-central
```

### 4. Set Your API Keys

Create a `.env` file with your API credentials:

```bash
# For OpenAI
export OPENAI_API_KEY=your_api_key_here

# For Anthropic
export ANTHROPIC_API_KEY=your_api_key_here

# For Gemini (optional)
export GOOGLE_API_KEY=your_api_key_here
```

Or set them directly in your shell:

```bash
export OPENAI_API_KEY=your_api_key_here
export ANTHROPIC_API_KEY=your_api_key_here
```

### 5. Run in Docker Sandbox (Recommended)

The safest way to run the agent is inside a Docker Sandbox:

```bash
# Source your environment variables
source .env

# Run the agent in a sandbox
docker sandbox run claude run container-nerdy-central.yaml
```

This command:
1. Creates an isolated container
2. Mounts your current workspace at the same path inside the container
3. Injects Git credentials automatically
4. Runs the cagent comedian safely

### 6. Interact with Your Comedian

Send a prompt to the comedian:

```bash
docker sandbox run claude run container-nerdy-central.yaml "Two container experts walk into a ..."
docker sandbox run claude run container-nerdy-central.yaml "Two agents discover the jetson orini nano developer kit..."

docker sandbox run claude run container-nerdy-central.yaml "what's funny about container names when they hit a kubernetes cluster on automode "
```
Note
If this is the first session, you will have to login: 
1. Authenticate into your Claude Account with Subscription

```bash
Prompt "what's funny about container names when they hit a kubernetes cluster on automode "
```

Or start an interactive session:

```bash
docker sandbox run claude run container-nerdy-central.yaml
# Then type your prompts when prompted
```

---

## 💻 Running Without Sandboxes (Standalone Mode)

If you prefer to run the agent directly without Docker Sandboxes:

### Basic Usage

```bash
source .env
./cagent run container-nerdy-central.yaml
```

### With Specific Prompts

```bash
source .env
./cagent run container-nerdy-central.yaml "Two container experts walk into a car lot..."
```

### With Debug Mode

```bash
source .env
./cagent run container-nerdy-central.yaml --debug
```

### Auto-approve Tool Calls (YOLO Mode)

```bash
source .env
./cagent run container-nerdy-central.yaml --yolo
```


---

## 🎯 Understanding the Agent Configuration

The `container-nerdy-central.yaml` file defines your AI comedian:

```yaml
version: "2"
agents:
  root:
    model: gpt-4o-mini
    description: "Container Comedy Central comedian"
    instruction: |
      You are a nerdy stand-up comedian specializing in
      Docker, Kubernetes, and container humor...

  backup_comedian:
    model: gpt-4o-mini
    description: "Backup comedian for failover"
    instruction: |
      You kick in when the main comedian has issues...
```

### Key Configuration Options

- **model**: AI model to use (gpt-4o-mini, claude-3-sonnet, etc.)
- **description**: Brief description of the agent's role
- **instruction**: Detailed persona and behavior guidelines
- **temperature**: Creativity level (0.0-1.0, default: 0.8 for comedy)

---

## 🎪 Example Interactions

### Getting Container Jokes

```bash
docker sandbox run claude run container-nerdy-central.yaml \
  "Tell me a joke about Docker container names"
```

**Response:**
> You know what's funny about Docker container names? They're always named things like "cranky_newton" or "silly_darwin". I mean, where's the love for "happy_hopper" or "joyful_javascript"? We need more positivity in our registry!

---

## 🔧 Advanced Configuration

### Override Model Settings

```bash
docker sandbox run claude run container-nerdy-central.yaml \
  --model openai/gpt-4o
```

### Use Specific Agent

If you have multiple agents defined:

```bash
docker sandbox run claude run container-nerdy-central.yaml \
  --agent backup_comedian
```

### Set Working Directory

```bash
docker sandbox run claude run container-nerdy-central.yaml \
  --working-dir /path/to/workspace
```

### Load Environment from File

```bash
docker sandbox run claude run container-nerdy-central.yaml \
  --env-from-file .env
```

---

## 🐳 Understanding Docker Sandboxes

### How It Works

When you run `docker sandbox run <command>`:

1. **Container Creation**: Docker creates an isolated container from a template image
2. **Workspace Mounting**: Your current directory is mounted at the same absolute path
3. **Credential Injection**: Git config (`user.name`, `user.email`) is automatically injected
4. **State Persistence**: One sandbox per workspace preserves state across sessions
5. **Execution**: Your command runs inside the container with controlled access

### Path Consistency Example

If your project is at:
```
/Users/alice/projects/cagent-comedy-central
```

It appears at the same path inside the sandbox:
```
/Users/alice/projects/cagent-comedy-central
```

This ensures error messages, logs, and hard-coded paths work correctly.

### What's Isolated

✅ **Protected from agents:**
- Your home directory (except the workspace)
- System files and directories
- Other projects outside the workspace
- SSH keys and credentials (unless explicitly mounted)
- Browser data and cookies

✅ **Available to agents:**
- Current workspace directory (mounted)
- Git credentials (read-only injection)
- Environment variables you provide
- Network access (if needed)

### Sandbox Persistence

Docker maintains one sandbox per workspace directory. This means:

- **Persists**: Installed packages, dependencies, configuration
- **Resets**: Temporary files outside the workspace
- **Shared**: Workspace file changes sync immediately

---

## 📚 Common Use Cases

### 1. Safe Experimentation

Test AI agents without risk to your host system:

```bash
docker sandbox run claude run container-nerdy-central.yaml \
  "Install a package and run some tests"
```

### 2. Team Collaboration

Share agent configurations that run identically for everyone:

```bash
# Push your agent configuration
cagent push ./container-nerdy-central.yaml youruser/comedy-central

# Team members pull and run
cagent pull youruser/comedy-central
docker sandbox run claude run comedy-central.yaml
```

### 3. CI/CD Integration

Run agents in sandboxes as part of your pipeline:

```yaml
# .github/workflows/test.yml
- name: Run AI Agent Tests
  run: |
    docker sandbox run claude run container-nerdy-central.yaml \
      "Run all tests and report results"
```

### 4. Development Workflow

Use the comedian to lighten the mood during development:

```bash
# When your build fails
docker sandbox run claude run container-nerdy-central.yaml \
  "My Kubernetes deployment just failed. Make me feel better."
```

---

## 🛡️ Security Best Practices

### 1. Always Use Sandboxes for Untrusted Agents

```bash
# Good - Isolated execution
docker sandbox run claude run untrusted-agent.yaml

# Direct host access
./cagent run untrusted-agent.yaml
```

### 2. Minimize Mounted Directories

Only mount what the agent needs:

```bash
cd specific-project
docker sandbox run claude run agent.yaml
# Only specific-project is mounted
```

### 3. Review Agent Configurations

Before running, check the `instruction` field in YAML files:

```bash
cat container-nerdy-central.yaml | grep -A 20 "instruction:"
```

### 4. Use Read-Only Mounts When Possible

If your agent only needs to read files:

```bash
docker sandbox run --read-only cagent run agent.yaml
```

### 5. Monitor Sandbox Activity

Enable debug logging to see what the agent is doing:

```bash
docker sandbox run claude run container-nerdy-central.yaml --debug
```

---

## 🔍 Troubleshooting

### Sandbox Not Starting

**Error**: `docker sandbox: command not found`

**Solution**: Ensure Docker Desktop 4.50+ is installed and Sandboxes feature is enabled.

```bash
docker --version  # Should be 4.50 or higher
```

### Agent Can't Find Files

**Error**: `No such file or directory`

**Solution**: Ensure you're running from the correct directory. The sandbox mounts your current working directory.

```bash
cd /path/to/cagent-comedy-central
docker sandbox run claude run container-nerdy-central.yaml
```

### Permission Denied

**Error**: `Permission denied` when accessing files

**Solution**: Check file permissions. The sandbox runs as your user but in a container context.

```bash
chmod +x cagent
chmod 644 container-nerdy-central.yaml
```

### API Key Not Found

**Error**: `API key not set`

**Solution**: Ensure environment variables are set before running:

```bash
source .env
docker sandbox run claude run container-nerdy-central.yaml
```

Or pass them explicitly:

```bash
docker sandbox run --env OPENAI_API_KEY=$OPENAI_API_KEY \
  cagent run container-nerdy-central.yaml
```

### Sandbox State Conflicts

If you need a fresh sandbox:

```bash
docker sandbox rm cagent-comedy-central
docker sandbox run claude run container-nerdy-central.yaml
```

---

## 🚀 Advanced Topics

### Creating Custom Agents

1. Copy the template:
```bash
cp container-nerdy-central.yaml my-agent.yaml
```

2. Edit the configuration:
```yaml
agents:
  root:
    model: gpt-4o-mini
    description: "My custom agent"
    instruction: |
      Your custom instructions here...
```

3. Run in a sandbox:
```bash
docker sandbox run claude run my-agent.yaml
```

### Multi-Agent Collaboration

Define multiple agents that can delegate to each other:

```yaml
agents:
  coordinator:
    model: gpt-4o
    description: "Routes tasks to specialists"

  specialist_a:
    model: gpt-4o-mini
    description: "Handles type A tasks"

  specialist_b:
    model: claude-3-sonnet
    description: "Handles type B tasks"
```

### Integrating MCP Tool Servers

Add external tools to your agents:

```yaml
agents:
  root:
    model: gpt-4o-mini
    tools:
      - type: mcp
        server: docker-server
        image: mcp-tools:latest
```

---

## 📖 Additional Resources

- [Docker Sandboxes Documentation](https://docs.docker.com/ai/sandboxes/)
- [Docker Blog: A New Approach for Coding Agent Safety](https://www.docker.com/blog/docker-sandboxes-a-new-approach-for-coding-agent-safety/)
- [cagent GitHub Repository](https://github.com/docker/cagent)
- [cagent Usage Guide](https://github.com/docker/cagent/blob/main/docs/USAGE.md)
- [MCP Toolkit & Catalog](https://docs.docker.com/ai/mcp-catalog-and-toolkit/toolkit/)

---

## 💡 Getting Help

### For cagent Issues

```bash
./cagent --help
./cagent run --help
```

### For Docker Sandboxes Issues

Check Docker Desktop logs:
```bash
docker info
docker sandbox list
```

### Report Issues

- **cagent bugs**: [GitHub Issues](https://github.com/docker/cagent/issues)
- **Docker Sandboxes bugs**:
  - [Mac](https://github.com/docker/for-mac/issues)
  - [Windows](https://github.com/docker/for-win/issues)
  - [Linux](https://github.com/docker/for-linux/issues)

### Community Feedback

```bash
./cagent feedback
```

---

## 🎉 Contributing

We welcome contributions! Whether it's:

- 🐛 Bug fixes
- ✨ New joke categories
- 📝 Documentation improvements
- 🔧 Configuration enhancements

Please open an issue or pull request!

---

## 📄 License

[Your License Here]

---

## 🙏 Acknowledgments

- Docker team for the amazing Sandboxes feature
- cagent developers for the multi-agent runtime
- The DevOps community for inspiration and endless container jokes

---

**Made with 🤖, 🐳, YAML, and a dash of humor!**

*Remember: Your agents are now containerized, just like your jokes—safely isolated but still accessible when you need them!*

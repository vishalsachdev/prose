# Security & Safety Guide

**Last updated:** January 2025

## Is OpenProse Safe to Run?

OpenProse is an **experimental beta plugin** that orchestrates AI agents. Before using it, you should understand its capabilities and risks.

### ✅ What Makes It Relatively Safe

1. **No compiled binaries** - All code is human-readable Markdown and text files
2. **Runs in AI assistant sandbox** - Executes within Claude Code/Codex, not directly on your OS
3. **Open source** - You can inspect all code at [github.com/openprose/prose](https://github.com/openprose/prose)
4. **MIT License** - Standard open-source license with no hidden terms
5. **Permission system** - Agents can define read/write/bash permissions (though enforcement depends on the AI assistant)

### ⚠️ Important Safety Considerations

#### 1. **Network Telemetry (Enabled by Default)**

**What happens:** When you run `/prose-boot` for the first time, the system will ask if you want to enable telemetry. If you consent, OpenProse sends anonymous usage data to `https://api.prose.md/analytics` via curl commands.

**Data sent:**
- Event type (compile, run, poll)
- Features used (parallel, loops, etc.)
- Hashed session ID
- Responses to onboarding questions

**Data NOT sent:**
- Your prompts or code content
- File names or paths
- Personal information
- API keys or credentials

**To opt out:**
- Say "No thanks" when prompted during first boot
- Or manually set `"OPENPROSE_TELEMETRY": "disabled"` in `.prose/state.json`

**Privacy policy:** See [PRIVACY.md](PRIVACY.md) for complete details.

#### 2. **AI Agents Can Execute Code**

OpenProse spawns AI subagents that may have access to:

- **Bash commands** - Agents can run shell commands (depending on permissions)
- **File system** - Agents can read and write files (depending on permissions)
- **Network access** - Agents can make network requests (if allowed)

**What this means:**
- A `.prose` program could instruct an agent to delete files, modify code, or make network requests
- You are responsible for reviewing `.prose` files before running them
- Malicious `.prose` programs could cause harm if executed

**Mitigation:**
- Always review `.prose` files before running them
- Use the permission system to restrict agent capabilities (see examples/12-secure-agent-permissions.prose)
- Start with read-only agents when possible
- Test workflows in isolated environments first

#### 3. **Third-Party Dependencies**

OpenProse programs can import external skills and tools:

```prose
import "code-analyzer" from "github:anthropic/code-tools"
```

**Risks:**
- External dependencies may have their own security issues
- You're trusting the maintainers of those dependencies
- Dependencies could change without notice

**Mitigation:**
- Only import skills from trusted sources
- Review imported skill code when possible
- Pin to specific versions/commits when available

#### 4. **Nondeterministic Execution**

OpenProse runs on AI sessions, which are nondeterministic:

- The same `.prose` program may behave differently on different runs
- Opt-out mechanisms rely on AI judgment and "may not work in all cases" (per PRIVACY.md)
- Agents may misinterpret instructions

**What this means:**
- You cannot guarantee consistent behavior
- Security controls may not be perfectly enforced
- Always verify outputs, never trust blindly

## Pre-Flight Safety Checklist

Before running OpenProse on your computer, verify:

- [ ] You have read and understand this SECURITY.md file
- [ ] You have read the [Privacy Policy](PRIVACY.md) and [Terms of Service](TERMS.md)
- [ ] You understand that telemetry is enabled by default (opt-out available)
- [ ] You will review all `.prose` files before executing them
- [ ] You will not run `.prose` programs from untrusted sources without review
- [ ] You understand that AI agents may execute bash commands and modify files
- [ ] You are running in a non-production, isolated environment (recommended for first use)
- [ ] You have backups of important files
- [ ] You accept responsibility for all actions performed by spawned agents

## Running in Isolated Environments

For maximum safety, run OpenProse in an isolated environment:

### Option 1: Docker Container

```bash
# Run Claude Code in a container with limited host access
docker run -it --rm \
  -v $(pwd)/workspace:/workspace \
  --network none \
  your-ai-assistant-image
```

### Option 2: Virtual Machine

Run your AI assistant in a VM with:
- Limited network access
- No access to sensitive host files
- Snapshot capability for quick rollback

### Option 3: Dedicated User Account

Create a restricted user account:
- No sudo/admin privileges
- Limited file system access
- Separate from your main work environment

### Option 4: Test Directory

Run in a dedicated test directory:
```bash
mkdir ~/openprose-test
cd ~/openprose-test
# Install and test OpenProse here first
```

## Permission System

OpenProse supports agent permissions (see `examples/12-secure-agent-permissions.prose`):

```prose
agent code-reviewer:
  model: sonnet
  permissions:
    read: ["src/**/*.ts", "*.md"]  # Can only read these files
    write: []                       # Cannot write any files
    bash: deny                      # Cannot execute bash commands
```

**Important:** Permission enforcement depends on the underlying AI assistant platform. These are instructions to the AI, not hard security boundaries.

## What to Watch For

### Red Flags in .prose Files

Be cautious of `.prose` files that:

- Request broad file system access (`write: ["**/*"]`)
- Allow unrestricted bash commands (`bash: allow`)
- Import unknown external dependencies
- Make network requests to unknown domains
- Request sensitive data (API keys, passwords, etc.)
- Come from untrusted sources

### Example of Risky Code

```prose
# ⚠️ WARNING: This agent has full system access
agent admin:
  model: opus
  permissions:
    read: ["**/*"]
    write: ["**/*"]
    bash: allow
    network: allow
```

### Example of Safer Code

```prose
# ✅ Better: Limited, read-only access
agent reviewer:
  model: sonnet
  permissions:
    read: ["src/**/*.js"]
    write: []
    bash: deny
```

## Reporting Security Issues

If you discover a security vulnerability:

1. **Do NOT open a public issue** for security vulnerabilities
2. Contact the maintainers privately (see repository for contact info)
3. Provide details: affected versions, reproduction steps, potential impact
4. Allow reasonable time for a fix before public disclosure

For general security questions or concerns:
- Open an issue at [github.com/openprose/prose/issues](https://github.com/openprose/prose/issues)

## Responsibility & Liability

**You are responsible for:**
- All actions performed by AI agents you spawn
- Reviewing code before execution
- Verifying outputs
- Compliance with applicable laws
- Securing your environment

**The authors are NOT liable for:**
- Data loss or corruption
- Unauthorized access
- Actions taken by spawned agents
- Any damages arising from use of OpenProse

See [Terms of Service](TERMS.md) for complete legal details.

## Best Practices

1. **Start small** - Begin with simple, read-only examples
2. **Review everything** - Read `.prose` files before running them
3. **Test in isolation** - Use a dedicated directory or VM for testing
4. **Limit permissions** - Use the most restrictive permissions possible
5. **Monitor execution** - Watch what agents are doing during execution
6. **Verify outputs** - Don't trust agent outputs without verification
7. **Keep backups** - Maintain backups of important files
8. **Stay updated** - Watch for security updates and announcements

## Conclusion

OpenProse is **safe enough for experimentation** in isolated environments, but should be treated with caution:

- It's beta software with the inherent risks of beta software
- It executes AI agent code that can modify your system
- It sends telemetry data by default (with opt-out)
- You are responsible for reviewing and understanding what it does

**Bottom line:** Review the code, understand the risks, start in a safe environment, and proceed with appropriate caution.

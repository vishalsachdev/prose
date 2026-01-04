# Quick Start Safety Checklist

Before running OpenProse for the first time, complete this checklist:

## Pre-Installation

- [ ] I have read [SECURITY.md](SECURITY.md)
- [ ] I have read [PRIVACY.md](PRIVACY.md)
- [ ] I have read [TERMS.md](TERMS.md)
- [ ] I understand this is beta software
- [ ] I understand AI agents can execute bash commands and modify files
- [ ] I understand telemetry is enabled by default (with opt-out)

## Installation Environment

Choose your safety level:

### Maximum Safety (Recommended for First Time)
- [ ] I'm running in a Docker container OR virtual machine
- [ ] The container/VM has limited network access
- [ ] The container/VM has no access to sensitive host files
- [ ] I have VM snapshots enabled for quick rollback

### Medium Safety
- [ ] I'm running in a dedicated test directory
- [ ] This directory contains no sensitive files
- [ ] I have backups of my important files
- [ ] I'm using a non-admin user account

### Minimum Safety (Not Recommended)
- [ ] I'm running in my normal work environment
- [ ] I understand and accept the risks
- [ ] I have complete backups
- [ ] I will carefully review every `.prose` file before running it

## First Steps

- [ ] Create a test directory: `mkdir ~/openprose-test && cd ~/openprose-test`
- [ ] Install the plugin
- [ ] Run `/prose-boot` and review the onboarding
- [ ] When prompted about telemetry, make an informed choice
- [ ] Start with `examples/01-hello-world.prose`
- [ ] Review `examples/12-secure-agent-permissions.prose` to understand permissions

## Before Running Any .prose File

- [ ] I have reviewed the entire `.prose` file
- [ ] I understand what each agent is configured to do
- [ ] I have checked the permissions (read, write, bash, network)
- [ ] I know which files/directories will be accessed
- [ ] I trust the source of this `.prose` file
- [ ] I have verified any external imports

## Red Flags to Watch For

Stop and reconsider if a `.prose` file:

- [ ] Has `write: ["**/*"]` (write access to all files)
- [ ] Has `bash: allow` without restrictions
- [ ] Imports unknown external dependencies
- [ ] Makes network requests to unfamiliar domains
- [ ] Comes from an untrusted source
- [ ] Requests sensitive information (API keys, passwords)

## After Running

- [ ] I have reviewed the outputs
- [ ] I have verified no unexpected files were created or modified
- [ ] I have checked for any unusual network activity
- [ ] If anything unexpected happened, I will report it

## Getting Help

- **Security issues:** Contact maintainers privately (see SECURITY.md)
- **General questions:** [github.com/openprose/prose/issues](https://github.com/openprose/prose/issues)
- **Documentation:** [skills/open-prose/docs.md](skills/open-prose/docs.md)

---

**Remember:** You are responsible for all actions performed by AI agents you spawn through OpenProse.

✅ Once you've completed this checklist, you're ready to explore OpenProse safely!

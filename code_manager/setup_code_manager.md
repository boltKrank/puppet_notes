# Setting up Puppet Code Manager:

## Setup summary:
1. Create control repo
2. Setup Puppetfiles in control repo
3. Configure code manager (or r10k)
4. Setup a deployment trigger to run code manager automatically
5. Use orchestrator to run puppet agents

### 1. Create control repo:

This is just creating a git repo with one branch per environment

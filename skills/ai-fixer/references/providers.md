# AI Provider Configuration

## Available Providers

### Claude CLI (Recommended)

Uses the local Claude CLI installation for AI fixes.

```bash
linthis fix --ai --provider claude-cli
```

**Requirements:**
- Claude CLI installed (`claude --version`)
- Authenticated with Anthropic

**Advantages:**
- No API key management
- Uses your existing Claude subscription
- Full context window
- Supports all Claude models

### Claude API

Direct API access to Claude.

```bash
linthis fix --ai --provider claude
```

**Configuration:**
```toml
[ai.claude]
api_key = "${ANTHROPIC_API_KEY}"  # or set env var
model = "claude-sonnet-4-20250514"
max_tokens = 4096
```

**Environment Variable:**
```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

### CodeBuddy CLI

Tencent's CodeBuddy AI assistant.

```bash
linthis fix --ai --provider codebuddy-cli
```

**Requirements:**
- CodeBuddy CLI installed
- Authenticated with Tencent Cloud

### Local LLM

Self-hosted language models.

```bash
linthis fix --ai --provider local
```

**Configuration:**
```toml
[ai.local]
endpoint = "http://localhost:11434/api/generate"
model = "codellama:13b"
```

## Provider Selection

| Provider | Speed | Quality | Privacy | Cost |
|----------|-------|---------|---------|------|
| claude-cli | Medium | Excellent | Cloud | Subscription |
| claude | Fast | Excellent | Cloud | Per-token |
| codebuddy-cli | Medium | Good | Cloud | Free tier |
| local | Varies | Good | Local | Hardware |

## Default Provider

Set in config:
```toml
[ai]
default_provider = "claude-cli"
```

Or via environment:
```bash
export LINTHIS_AI_PROVIDER="claude-cli"
```

## Provider-Specific Options

### Claude CLI Options

```toml
[ai.claude-cli]
# Additional CLI arguments
extra_args = ["--model", "claude-sonnet-4-20250514"]
# Timeout in seconds
timeout = 300
```

### Prompt Customization

```toml
[ai]
# Custom system prompt prefix
system_prompt_prefix = """
You are a code quality expert. When fixing issues:
1. Preserve existing code style
2. Add comments explaining complex fixes
3. Prefer minimal changes
"""
```

## Troubleshooting

### Claude CLI Not Found
```bash
# Check installation
which claude
claude --version

# Install if missing
npm install -g @anthropic-ai/claude-cli
```

### API Key Issues
```bash
# Verify key is set
echo $ANTHROPIC_API_KEY

# Test API access
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "content-type: application/json" \
  -d '{"model":"claude-sonnet-4-20250514","max_tokens":10,"messages":[{"role":"user","content":"Hi"}]}'
```

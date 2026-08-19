# Opencode Agent Config

Agent configuration to delegate everyday mundane tasks to Qwen3.8
running on my RTX 3090 GPU, and more demanding development work to
OpenAI models via my $20/month subscription.  Sol handles the complex
problem-solving and architectural reasoning, and Luna is used as an
escalation path for subagents handling tasks that are more challenging
than what Qwen3.8 local (4-bit quantized) can handle.

## Config Repository Structure

```
                       AGENTS.md
                 rules everyone follows
                          │
              ┌───────────┴───────────┐
              │                       │
      orchestrator.md             project AGENTS.md
      "how to manage work"        "how this repo works"
              │
       ┌──────┼──────────┐
       ▼      ▼          ▼
 explorer   worker    escalation
   .md       .md          .md
 "research" "implement"  "solve hard stuff"
```

## Orchestration Hierarchy

```
                            GPT-5.6 Sol
                            orchestrator
                                 │
                  ┌──────────────┼──────────────┐
                  ▼              ▼              ▼
           Local Explorer   Local Worker   Cloud Escalation
           Qwen3.8-27B      Qwen3.8-27B          │
            read-only        routine SWE         │
                                                  ▼
                                             Luna High
                                             difficult
                                                  │
                                     if unresolved/insufficient
                                                  │
                                                  ▼
                                            back to Sol
                                                  │
                                                  ▼
                                            Luna xHigh
                                           very difficult
                                                  │
                                     if unresolved/insufficient
                                                  │
                                                  ▼
                                            back to Sol
                                                  │
                                                  ▼
                                             Luna Max
                                            exceptional

                  └──────────────┬──────────────┘
                                 ▼
                       Sol reviews, integrates,
                         verifies, and delivers
```

The orchestrator invokes every tier directly. Escalation is progressive in
the orchestrator's selection policy; subagents do not invoke other subagents.

## Setup

From the cloned repository:

```shell
repo_dir="$(pwd)"
config_dir="$HOME/.config/opencode"

mkdir -p "$config_dir"
ln -s "$repo_dir/AGENTS.md" "$config_dir/AGENTS.md"
ln -s "$repo_dir/agents" "$config_dir/agents"
ollama pull qwen3.8:27b

# OpenCode requires at least a 64K context window for local coding agents. If
# Ollama is not managed by the desktop app or a system service, start it with:
export OLLAMA_CONTEXT_LENGTH=64000 # in .bashrc or someplace like that

# check if ollama is running
curl http://127.0.0.1:11434/api/tags

# if not running:
systemctl ollama start
# or
ollama serve
```

Register the local model and select the orchestrator in
`~/.config/opencode/opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "orchestrator",
  "subagent_depth": 1,
  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen3.8:27b": {
          "name": "Qwen3.8 27B"
        }
      }
    }
  }
}
```

## Run

Ensure Ollama is running, then start OpenCode and run `/connect`. Select
**OpenAI**, choose **ChatGPT Plus/Pro**, and complete browser authentication so
the orchestrator and cloud escalation agents can use the OpenAI models.

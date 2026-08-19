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
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
      Local Explorer     Local Worker    Cloud Escalation 1
      Qwen3.8-27B        Qwen3.8-27B        Luna High
       read-only          routine SWE       difficult
             │                │                │
             │                │                ▼
             │                │         Cloud Escalation 2
             │                │             Luna xHigh
             │                │            very difficult
             │                │                │
             │                │                ▼
             │                │         Cloud Escalation 3
             │                │              Luna Max
             │                │            exceptional
             │                │
             └────────────────┴───────────────┘
                              │
                              ▼
                    Sol reviews, integrates,
                      verifies, and delivers
```

## setup

``` shell
$ cd ~/$projects_dir/opencode-agent-config
$ cd ~/.config/opencode
$ ln -s ../$projects_dir/opencode-agent-config/AGENTS.md .
$ ln -s ../$projects_dir/opencode-agent-config/agents .
```

in ~/.config/opencode/opencode.json:
```
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "orchestrator"
}
```

## run

``` shell
$ ollama launch opencode
```

# Opencode Agent Config

```
                    GPT-5.6 Sol
                    orchestrator
                         │
          ┌──────────────┼───────────────┐
          ▼              ▼               ▼
     Qwen3.8 local    Luna Low        Luna High
       routine        reliable         difficult
                                          │
                                          ▼
                                      Luna Max
                                     exceptional
```

Agent configuration to delegate everyday mundane tasks to Qwen3.8
running on my RTX 3090 GPU, and more demanding development work to
OpenAI models via my $20/month subscription.  Sol handles the complex
problem-solving and architectural reasoning, and Luna is used as an
escalation path for subagents handling tasks that are more challenging
than what Qwen3.8 local (4-bit quantized) can handle.

## setup

``` shell
$ cd ~/$projects_dir/opencode-agent-config
$ cd ~/.config/opencode
$ ln -s ../$projects_dir/opencode-agent-config/* .
```

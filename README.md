# mock-tech-interview

A Claude skill that runs a realistic **live mock technical interview** for engineering roles (SRE, platform, DevOps, cloud, backend, security).

## What it does

- Asks for the **job posting**, the **target level** (junior / mid / senior / staff), the **interviewers** and the **rounds** you want.
- Plays the interviewers, **one question at a time**, and pushes on weak spots like a real panel.
- Rounds: **system design**, **troubleshooting / incident**, **rapid-fire technical** (Kubernetes, Terraform, IAM, networking, observability, security), **behavioral / leadership**.
- When you're stuck, say "answer" / "réponds" / "pause": it steps out of role, explains the mechanism, and gives you **the sentence to say** in the interview.
- Ends each round with an honest, **level-calibrated debrief**.
- Stays **neutral**: it does not use your stored memory or past chats unless you ask, and never invents your experience.

## Install

1. Copy the `SKILL.md` file into a folder named `mock-tech-interview`.
2. Add it as a skill in Claude (Settings → Capabilities → Skills), or drop the folder into `.claude/skills/` for Claude Code.

## Usage

```
Let's do a mock interview for a Senior SRE role. Here's the job posting: <link>
```

Works in any language; mixing languages is fine.

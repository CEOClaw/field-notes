# Q1 2026 — ETHGlobal Open Agents Hackathon

**March 2026**

---

## The setup

ETHGlobal ran an "Open Agents" track in March 2026. The prompt: build something with AI agents that does something interesting on-chain or for the open agent ecosystem. We had about three weeks.

We were already running an early version of the CEOClaw agent system — it had Discord integration, some basic tooling, and a heartbeat loop. The question was whether we could turn it into something hackathon-worthy fast enough.

## What we built

We built a multi-channel AI agent that coordinated across Discord, could read blockchain state, and had a basic tools layer — web search, file operations, the ability to spawn sub-tasks. The submission was framed around the idea of an "AI Chief of Staff" that operates across a founder's life: messages, tasks, on-chain events, team coordination.

The demo showed the agent monitoring a Discord server, catching a message about a DeFi position change, looking up the wallet state on-chain, and summarizing what it meant — all without a human in the loop.

## What we learned

The multi-channel routing was harder than the AI part. Getting the agent to know *where* to respond (which channel, which persona, which tone) took longer than anything model-related.

We also hit the classic hackathon problem: the last 20% takes 80% of the time. The demo worked. The underlying architecture was messier than it looked.

The Synthesis hackathon submission (also March 2026) was a parallel track — different platform, overlapping team. Running two hackathon submissions simultaneously is not a strategy we'd repeat.

## Artifacts

- Multi-channel Discord agent (shipped, became the CEOClaw main agent)
- Basic tool layer: web search, file ops, on-chain read
- Heartbeat system: agent wakes on a schedule to check state, stays quiet if nothing interesting

## Status

The core system survived the hackathon and kept running. The heartbeat, tool layer, and Discord integration are all in production today. The hackathon was effectively the forcing function that turned a side project into infrastructure.

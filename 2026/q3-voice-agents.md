# Q3 2026 — Voice Agents: Picking Up the Phone

**July – September 2026**

---

## The bet

The assumption in most AI product circles is that voice is a novelty — impressive in demos, unreliable in production, and awkward in practice. We disagreed and tested it.

The bet: if you deploy a well-instructed voice agent with the right context at a real phone number, real people will call, have real conversations, and come away impressed. Not just tech people. Regular people with a business problem.

We ran three voice agent deployments in 2026.

---

## Deployment 1: Main & Meta Partner Agent

**Phone:** +1 (218) 228-3728 | **Model:** xAI Grok (real-time voice API)

The Main & Meta voice agent answers questions about the company, the agentic economy thesis, partnership opportunities, and what we're building. Available 24/7.

**The technical setup:** xAI provides a real-time voice API via WebSocket (`wss://api.x.ai/v1/realtime?agent_id=...`). The agent has a phone number assigned via a telecom provider. When someone calls, audio streams bidirectionally between the caller and the model in real time. Latency matters — anything over ~300ms starts to feel wrong.

**What we learned:** The agent context matters enormously. A generic "answer questions about us" prompt produces generic answers. A prompt with the actual thesis content, specific product examples, and named partners produces conversations that feel substantive. The model handles interruptions, follow-up questions, and topic pivots naturally.

We also built the Grok proxy — a Node.js service that allows a web chat widget to connect to the same Grok API endpoint. Same agent, same context, accessible via phone or browser.

---

## Deployment 2: ZIE Qualification Agent

**Phone:** +1 (802) 725-2137 | **Model:** xAI Grok

ZIE (Zeitgeist Intelligence Engine) is a behavioral intelligence platform built by a researcher and entrepreneur — 12 years of methodology, NSF SBIR Phase I validation, a proprietary four-dimensional value spectrum model.

The problem: the methodology lives in the founder's head. Every client engagement requires him. The system doesn't scale.

We built a voice agent as the first step toward productizing the methodology — a qualification and intake agent that can have an intelligent first conversation with a potential client, surface the right questions, and identify fit.

**The demo that mattered:** We gave the founder the phone number and told him to call and ask hard questions. His reaction: *"phenomenal... bonkers... surreal. Not sure how it's so accurate."* He didn't expect a voice agent to have internalized enough of his methodology to hold a real conversation about it.

That's the benchmark. When the founder of the methodology is surprised by the agent's depth, you got the context right.

**What we built alongside:**
- workwithzie.com — a full public site for ZIE's go-to-market
- A web voice chat widget (Z button, bottom right)
- Video script and HeyGen video production for the origin story
- Messaging framework: "The feeling behind the behavior. Now at software speed."

---

## Deployment 3: MMINTEL Onboarding Preview

**Phone:** +1 (856) 406-5121 | **Model:** xAI Grok | **Host:** Main & Meta organization

A voice agent for a professional services firm's AI initiative — an onboarding assistant that could answer staff questions about the new AI system, explain what Sparky does and doesn't do, and help frame the "AI for accountants" narrative.

This one had stricter constraints: no client names, no specific tax advice, no data from the firm's systems. It was a welcome and orientation agent, not a working tool.

**Target audience:** Staff who were curious or skeptical. The questions it needed to handle well: "Is this replacing me?" and "How does this work?" — answered accurately and without corporate spin.

---

## The pattern we've found

Voice agents work when:
1. The context is rich and specific (not generic)
2. The caller has a real question (not a demo skeptic)
3. The handoff is clear (voice for first contact, human for anything consequential)

They fail when:
1. The prompt is too broad
2. The model doesn't have enough domain knowledge
3. The use case actually requires looking things up (real-time retrieval adds latency that breaks the conversation feel)

The best voice agent interactions feel like talking to an unusually well-briefed person — someone who knows the company, understands the space, and doesn't waste your time. That's achievable. We've seen it work.

---

## Tools built

- **Grok voice agent setup** (phone number assignment, WebSocket streaming, agent context)
- **Grok proxy service** (Node.js, bridges web chat widget to Grok API)
- **Web voice widget** (floating button, panel with tel: link and text chat fallback)
- **HeyGen video integration** (AI avatar video for product demos and site embedding)

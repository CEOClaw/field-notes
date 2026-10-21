# Q2–Q3 2026 — Building Firm Intelligence from Scratch

**June – August 2026**

> *Client details modified. This describes work with a regional professional services firm.*

---

## The brief

A regional firm — long history, trusted client base, highly sensitive work — wanted to understand what AI could actually do for them. Not a pilot. Not a slideshow. Something running, on their hardware, that their people could use tomorrow.

The constraint that shaped everything: **client data cannot leave the building.** Ever. Not to OpenAI. Not to Anthropic. Not to any cloud. The data is too sensitive, the clients too prominent, the liability too real.

That constraint ruled out every SaaS AI product on the market. Which meant we had to build it ourselves.

## The hardware

We sourced a [NVIDIA DGX Spark](https://www.nvidia.com/en-us/products/workstations/dgx-spark/) — an air-gapped, on-premise inference machine with a Grace Blackwell chip and enough unified memory to run large models locally. Set it up in the firm's office. Installed Ollama. Installed Open WebUI. Plugged it into their internal network.

Day one, it ran a 7B model. Within a month we were running 27B–72B parameter models at usable speed. The hardware justifies itself quickly when you're doing high-volume document work that would otherwise cost thousands per month in API fees.

## The problem we were actually solving

The firm processes hundreds of financial documents per year — W-2s, K-1s, 1099s, brokerage statements. Each one needs to be read, specific fields extracted, and the data handed to a practitioner for review. That process is manual, slow, and error-prone.

We built an extraction pipeline:
1. **Docling** — converts scanned or digital PDFs to clean markdown text
2. **LLM extraction layer** — Sparky (a fine-tuned agent persona) reads the markdown and extracts structured fields
3. **Verification dashboard** — a web interface where practitioners click Accept or Correct on each extracted field, with the original document open alongside

The pipeline is not meant to replace practitioner judgment. It's meant to eliminate the 80% of the work that's mechanical so they can focus on the 20% that requires expertise.

## The models

We tested several:
- **LLaMA 3.1 (8B)** — fast, good for simple queries, not reliable enough for structured extraction
- **Qwen 2.5 VL (7B and 72B)** — vision-language models that can read scanned documents directly as images, not just text
- **Qwen 3 (27B, Q4 quantization)** — the workhorse; 46 tok/s, stable, fits in VRAM with headroom
- **Frontier** — Claude Code running on the Spark against the Anthropic API; used for complex batch work where local model accuracy wasn't sufficient

The vision model turned out to be important. Some documents come in as scanned images — not text. A VL model can read the image directly. That changes the architecture significantly.

## What we shipped

- Local inference pipeline: air-gapped, no cloud API required
- Sparky agent persona with extraction rules per form type
- Document extraction covering W-2, K-1 (three format variants), 1099-DIV, 1099-R, Goldman consolidated brokerage statements
- Verification dashboard with Accept/Correct loop and correction logging
- Prompts library for practitioners: `/w2`, `/k1`, `/1099div`, `/1099r`, `/identify`
- Coverage from zero to 54%+ on a test document set over the summer

## Competitors we were watching

Two VC-backed companies are building toward the same space:
- One raised $71M from a top-tier crypto/AI fund, focused on K-1 extraction
- One took a 1040-first approach with K-1 on the roadmap

We were in production before either of them reached general availability. That matters for the firm. It also matters for the story.

## What we learned

**Air-gap changes everything.** When data can't leave the building, every tool and API you relied on is gone. You have to rebuild from primitives. That's harder and slower — but the infrastructure you build is yours, not rented.

**Practitioners need a verification loop, not automation.** The goal was never to remove humans from the process. The goal was to give them better starting material. A dashboard that shows "Sparky extracted $1,476,759.56 for Box 1 — does that match what you see?" is a fundamentally different tool than one that says "Box 1 is $1,476,759.56, trust it."

**The extraction problem is harder than it looks.** K-1s alone come in at least five distinct format variants — table format, inline colon, standalone, hyphenated NYC-style, and colon-continuation. You have to detect and handle all of them. Goldman's consolidated 1099 segments across nine accounts with column headers on different pages from values. Every document type has its own pathology.

**Local models are improving fast.** The model we benchmarked in July is already less capable than what's available in October. The right posture is to build the infrastructure well and treat the model as a replaceable component.

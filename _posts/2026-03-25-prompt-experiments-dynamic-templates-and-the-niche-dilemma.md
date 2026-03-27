---
layout: post
title: "Devlog #2: Prompt Sensitivity and the Discovery of Dynamic Templates"
published: false
date: 2026-04-25
categories: [ai, engineering, building-in-public]
---

With the infrastructure for MagicExtract now stable, I’ve moved into testing one of my riskier assumptions. In the 2026 landscape, we often assume LLMs are "smart enough" to handle whatever we throw at them, but the reality is that extraction quality is still incredibly sensitive to prompt structure and document context.

### The Impact of Prompt Tuning
I spent most of today benchmarking how different prompt variations affect both extraction accuracy and latency. 

It’s a balancing act: 
1. **The "Verbose" approach:** Giving the model deep context about every possible edge case improves accuracy but adds 400-600ms of latency and increases token costs.
2. **The "Lean" approach:** Trusting the model's internal weights for standard fields like `total_amount` is fast, but it fails on documents with complex tax breakdowns.

I’ve started building a structured testing suite to quantify these trade-offs. The goal is to find the "Goldilocks" prompt—minimum tokens for maximum reliability.

### Moving Beyond Static Schemas
The most interesting breakthrough today was the implementation of **Dynamic Template Discovery**.

Initially, the system required a pre-defined list of fields. But I realized that the model often "sees" valuable data that I haven't asked for. I updated the extraction logic to distinguish between the **target document type** (e.g., a Quote vs. a generic Invoice) and the **requested fields**.

Now, if I tell MagicExtract it’s looking at a "Quote," the extractor doesn't just pull the fields I defined; it flags additional relevant fields it discovers—like "Validity Period" or "Project Lead"—and suggests adding them to the template.

It’s a feedback loop:
* The human provides a starting hint.
* The AI suggests refinements based on what it actually finds in the data.
* The template evolves without me having to manually map out every possibility.

### The "Generic Tool" Trap
While the technical side is clicking, the business side is where I’m currently stuck. MagicExtract is, by design, very generic. It can handle medical records, construction quotes, or shipping manifests. 

However, "generic" is a hard sell. In a world where AI agents are everywhere, a tool that does everything often ends up doing nothing for anyone in particular. I’m currently weighing whether to niche down early—perhaps into **Logistics** or **Legal Tech**—to build a specific "Hint Library" that resonates with a clear user base.

### What's Next?
I’m expanding the test document set to way more varied PDFs. I need to see exactly where the "Hint-based" logic breaks and what algorithmic changes move the needle on extraction confidence scores.

I’ll be focusing on the benchmarking results for the next update.

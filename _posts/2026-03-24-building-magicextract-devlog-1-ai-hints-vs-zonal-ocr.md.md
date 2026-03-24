---
layout: post
title: "Devlog #1: Why I'm Building MagicExtract (And the Prompt That Started It)"
date: 2026-03-24
categories: [ai, engineering, building-in-public]
---

I’ve spent quite some time building data extraction tools over the years. 

I know the "old way" brittle way of coordinate-based mapping, regex patterns, and the constant maintenance every time a document layout shifts by a few pixels.

I also <a href="https://www.youtube.com/watch?v=NNHCbIgSVE8" target="_blank">built applications</a> in the "current" LLM based way, which feels a lot lee brittle, but I wanted to see how far we can push it. 

I wanted to know: Can we build something that moves past "reading text" and into "understanding intent"? I wanted an intuitive and highly dynamic system.

That’s the experiment behind **MagicExtract**.

### The Motivation
My goal isn't just to build another parser. I want to see if we can create a system that is fundamentally more resilient. Instead of rigid schemas, I’m betting on **Natural Language Hints**. 

When I explain a document to a human, I don't give them a JSON schema. I say: *"The Tax ID is usually in the top right, but if it's a utility bill, look near the footer."* I’m building MagicExtract to work the same way. You define what you need, provide a few hints, and let the model handle the semantic heavy lifting. 

It’s an experiment in seeing if "hint-based" extraction can handle the messy, unpredictable reality of real-world documents across dozens of different use cases.

### Building for an Uncertain Future
We don't know exactly how software will be consumed in two years, so I’m trying to make this as future-proof as possible by launching with three distinct "front doors":

1.  **The UI:** A traditional dashboard for humans to manage and verify data.
2.  **The API:** A robust REST interface for developers to plug into their existing apps.
3.  **The Agent Interface (MCP):** This is the one I’m most curious about. By supporting the Model Context Protocol, MagicExtract can "talk" directly to other AI agents. If a user’s local AI assistant needs to process an invoice, it can just query my service as a tool.

### The Technical Foundation
To move fast without sacrificing security, I chose a stack that prioritizes pragmatism:

* **Next.js 16:** Super useful for LLM/Agent-first coding. Using Server Actions allows me to keep the UI and the backend logic tightly coupled and type-safe.
* **Supabase:** Is reliable, flexable and secure. In a multi-tenant app, data isolation isn't something you want to "hope" you got right in the code. RLS locks it down at the database level.

### The First Prompt
I’m building this project in **Cursor**, and I’ve found that the best way to keep the AI on track is to define the "Source of Truth" before ever touching the UI. Here is the prompt I used to kick off the project:

> *"I want to start Phase 1 of MagicExtract. Please perform the following steps: 
> 1. Initialize a Next.js 16 project with TypeScript, Tailwind CSS, and Shadcn UI. 
> 2. Set up Drizzle ORM and postgres. Define the core schema for: Organizations (Tenants), Profiles (Users), and Extraction Templates. 
> 3. Every table MUST have a tenant_id. Write the SQL for Row Level Security (RLS) as a comment so I can apply it to Supabase. 
> 4. Don't build UI yet—just the foundation."*

### Current Status
The database is live, the RLS policies are active, and I’ve just finished the UI for creating these "Hint-based" templates. It’s a clean, minimalist start. 

The next step is the real test: feeding the first few PDFs into the LLM router and seeing if these hints actually hold up against the "messy" documents they were designed for. 

I’m documenting this journey because I’m genuinely curious about where this tech ends. Whether this becomes a standalone product or a component for a hundred other use cases, the goal is to see what happens when we stop drawing boxes and start giving hints.

I'll be sharing more as the first documents start flowing through the system.

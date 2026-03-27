---
layout: post
title: "Devlog #1: Why I'm Building MagicExtract (And the Prompt That Started It)"
published: false
date: 2026-04-24
categories: [ai, engineering, building-in-public]
---

I’ve spent quite some time building data extraction tools over the years. 

I know the "old" brittle way of coordinate-based mapping, regex patterns, and the constant maintenance every time a document layout shifts.

I also <a href="https://www.youtube.com/watch?v=NNHCbIgSVE8" target="_blank">built applications</a> in the "current" LLM based way, which feels a lot less brittle, but I wanted to see how far we can push it. 

I wanted to know: Can we build something that moves past "reading text" and into "understanding intent"? I wanted an intuitive and highly dynamic system.

That’s the experiment behind **MagicExtract**.

### The Motivation
My goal isn't just to build another parser. I want to see if we can create a system that is fundamentally more resilient. Instead of rigid schemas, I’m betting on **Natural Language Hints**. 

When I explain a document to a human, I don't give them a JSON schema. I say: *"The Tax ID is usually in the top right, but if it's a utility bill, look near the footer."* I’m building MagicExtract to work the same way. You define what you need, provide a few hints, and let the model handle the semantic heavy lifting. 

It’s an experiment in seeing if "hint-based" extraction can handle the messy, unpredictable reality of real-world documents across dozens of different use cases.

### Building for an Uncertain Future
We don't know exactly how software will be consumed in two years, so I’m trying to make this as future-proof as possible by launching with three distinct "consumption interfaces" in mind:

1.  **A UI:** A traditional dashboard for humans to manage and verify data.
2.  **An API:** A robust REST interface for developers to plug into their existing apps.
3.  **An Agent Interface (MCP):** This is the one I’m most curious about. By supporting the Model Context Protocol, MagicExtract can "talk" directly to other AI agents. If a user’s local AI assistant needs to process an invoice, it can just query my service as a tool.

### The Technical Foundation
To move fast without sacrificing security, I chose a stack that prioritizes pragmatism:

* **Next.js 16:** Super useful for LLM/Agent-first coding. Using Server Actions allows me to keep the UI and the backend logic tightly coupled and type-safe.
* **Supabase:** Is reliable, flexable and secure. In a multi-tenant app, data isolation isn't something you want to "hope" you got right in the code. RLS locks it down at the database level. Also superbase is very well "understood" by LLMs.

### The First Prompt
I’m building this project in **Cursor**, and I’ve found that the best way to keep the AI on track is to define the "Source of Truth" before implementing the UI. Here is the prompt I used to kick off the project:

> "I want to start Phase 1 of MagicExtract. Please perform the following steps: 
> 1. Initialize a Next.js 16 project with TypeScript, Tailwind CSS, and Shadcn UI. 
> 2. Set up Drizzle ORM and postgres. Define the core schema for: Organizations (Tenants), Profiles (Users), and Extraction Templates. 
> 3. Every table MUST have a tenant_id. Write the SQL for Row Level Security (RLS) as a comment so I can apply it to Supabase. 
> 4. Don't build UI yet—just the foundation."*

Here is my .cursorrules file:
```
# Project: MagicExtract
# Role: Expert Full-Stack AI Engineer (TypeScript & Supabase Specialist)

## 🎯 Project Context
You are building 'magicextract', a multi-tenant AI SaaS for hint-based PDF data extraction.
The core value prop is "Natural Language Training" where users give 'hints' to improve extraction without writing code or schemas.

## 🛠 Tech Stack (2026 Standards)
- **Framework:** Next.js 16 (App Router, Server Actions, Strict TS).
- **Database/Auth:** Supabase (PostgreSQL, Auth, RLS, pgvector, Vault).
- **ORM:** Drizzle ORM (for type-safe migrations and queries).
- **AI:** Vercel AI SDK (with `generateObject` and Zod).
- **UI:** Tailwind CSS, Shadcn UI, Lucide Icons.
- **Validation:** Zod (Every API and AI output must be validated).

## 🏢 Multi-Tenancy & Security Rules
- **RLS First:** Every table MUST have a `tenant_id` column. Never write a query without checking for Tenant isolation.
- **Secret Management:** User API keys (BYOK) must never be stored in plain text. Use Supabase Vault or encrypted columns.
- **Organization Scoping:** Users belong to Organizations. Data access is scoped to `organization_id` or `tenant_id`.

## 🤖 AI & Extraction Logic
- **Dynamic Schemas:** We don't use hardcoded JSON schemas. We generate Zod objects dynamically from an array of strings provided by the user.
- **Prompt Engineering:** Use the 'System Prompt + User Hint + Document Text' structure.
- **Provider Agnostic:** Use the Vercel AI SDK's unified interface. Implement logic to switch models based on `tenant_tier` (e.g., 'gpt-4o' for Pro, 'gemini-1.5-flash' for Basic).

## 💻 Coding Standards
- **Strict TypeScript:** No `any`. Use interfaces/types for all data structures.
- **Server-First:** Prefer React Server Components (RSC) and Server Actions over client-side fetching. Use Server Actions for all data mutations
- **Component Style:** Functional components, 'use client' only when necessary (interactive UI), and Shadcn for primitives.
- **Error Handling:** Use a robust Error Boundary and structured logging for LLM failures.

## 📍 Step-by-Step Instructions
- Refer to @README.md for the current Phase.
- DO NOT skip ahead to future phases unless explicitly asked.
- When generating code, ensure it is modular and "Agent-friendly" (clean internal APIs).

## 📂 File Structure Conventions
- `/app`: Next.js App Router.
- `/db`: Drizzle schemas and migrations.
- `/lib/ai`: Extraction logic and prompt templates.
- `/components/ui`: Shadcn primitives.
- `/hooks`: Custom React hooks for UI state.

## 🤖 Agentic & Discovery Rules (2026 Standards)
- **MCP Native:** All core extraction logic must be wrapped as 'Tools' that can be exported to an MCP Server. Use the `@modelcontextprotocol/sdk`.
- **Self-Documenting Functions:** Use JSDoc for all exported functions. Include `@agent-description` tags so Cursor and external agents understand *when* and *why* to call a function.
- **Discovery Endpoint:** Maintain a `/.well-known/ai-plugin.json` and a `/api/mcp` endpoint that describes all available templates and their schemas dynamically.

## 🧠 AI SDK 6 Best Practices
- **Agent Abstraction:** Use the `Agent` or `ToolLoopAgent` classes from Vercel AI SDK 6 for long-running extraction tasks that require multiple steps (e.g., OCR -> Classify -> Extract).
- **Strict Structured Output:** Use `generateObject` with `output: Output.object({ schema: ... })` to ensure 100% valid JSON. Never rely on raw text parsing for data extraction.

## 🔒 Security & Supabase Advanced
- **BYOK via Vault:** Never store provider keys in public/private tables. Use the `supabase.vault` schema for all third-party API keys (OpenAI, AWS, etc.).
- **RLS Verification:** Before creating any new table, generate the SQL `ALTER TABLE ... ENABLE ROW LEVEL SECURITY` and at least one `CREATE POLICY` statement to ensure tenant isolation.

## 🛠️ Drizzle & Migrations
- **Workflow:** Always use `drizzle-kit generate` for changes. Do not manually edit the database via the Supabase UI.
- **Type Safety:** Use `InferSelectModel` and `InferInsertModel` from `drizzle-orm` to keep your frontend types in sync with the DB.
```

### Current Status
The database is live, the RLS policies are active, and I’ve just finished the UI for creating these "Hint-based" templates. It’s a clean, minimalist start. 

The next step is the real test: feeding the first few PDFs into the LLM router and seeing if these hints actually hold up against the "messy" documents they were designed for. 

I’m documenting this journey because I’m genuinely curious about where this tech ends. Whether this becomes a standalone product or a component for a hundred other use cases, the goal is to see what happens when we stop drawing boxes and start giving hints.

I'll be sharing more as the first documents start flowing through the system.

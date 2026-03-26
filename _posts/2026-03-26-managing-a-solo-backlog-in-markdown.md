---
layout: post
title: "Devlog #3: Keeping an AI workflow first backlog (lean)"
date: 2026-03-25
categories: [ai, engineering, building-in-public]
---

Since I’m working solo on MagicExtract, I’ve moved my backlog out of external tools like Jira and directly into the repository. For a one-person project, those platforms feel like unnecessary overhead. Instead, I keep everything in a `/docs` directory.

The real benefit is context. When I'm working with an agent like Cursor, having the requirements in a browser tab is just a friction point. If the context isn't in the repo, the AI can't "see" it.

My current workflow:

- inbox.md: A raw brain dump for any feature idea or edge case.

- /docs/features/*.md: When I'm ready to build, I create a spec that defines the Zod schema and Supabase RLS changes first.

By pointing the agent to these files with the @ symbol, I’m providing a "technical contract" before any code is written. 

It ensures the AI understands the intent and the data structures without me having to copy-paste requirements constantly.

Keeping the backlog in Markdown also means it’s version-controlled. 

If I revert a commit, the documentation reverts with it. 

It’s a pragmatic way to keep the plan and the code in sync while making the most of the "Agent+Human" loop.

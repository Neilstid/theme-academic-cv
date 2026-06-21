---
title: Argos
date: 2026-06-20
external_link: https://github.com/Neilstid/argos
tags:
  - Python
  - LLM
  - AI
  - RSS
---

# Description

Argos is an AI-powered news blog generator. It automatically collects articles 
from configured RSS feeds, processes and summarizes them using LLM agents, 
and compiles them into a clean Markdown blog post.

This includes:
- **RSS Feed Collection**: Parses and extracts content from multiple RSS feeds.
- **LLM Processing**: Utilizes `crewai` and `litellm` (with Mistral by default) 
to map and reduce articles, summarizing and selecting the most relevant news.
- **Markdown Output**: Generates a ready-to-publish Markdown file.
- **Configurable**: Define your feed sources in simple `.yaml` files.

# Project Brief

## Overview
Gepetto is an open-source Python plugin for Hex-Rays IDA Pro (≥ 7.4) that leverages modern Large Language Models (LLMs) to enrich the reverse-engineering workflow. By sending decompiled function listings to an LLM, Gepetto can:

1. Explain the purpose and behaviour of a function in natural language.
2. Suggest more meaningful names for local variables and the function itself.
3. (Experimental) Generate equivalent, compilable C or Python code snippets.
4. Offer a Chat-style CLI inside IDA for arbitrary questions about the current binary.

These capabilities help reverse engineers understand unfamiliar code quickly, reduce cognitive load, and speed up routine renaming/documentation tasks.

## Core Requirements & Goals
• Seamless integration with IDA Pro UI and Hex-Rays decompiler output.
• Support multiple LLM providers (OpenAI, Azure OpenAI, Groq, Together, NovitaAI, Ollama, Kluster.ai, LM Studio, etc.).
• Allow user to choose the active model at runtime via an IDA menu.
• Minimise latency by running model selection and provider discovery on background threads.
• Internationalisation (i18n) with gettext-based locale switching.
• Configuration via `gepetto/config.ini`, environment variables fallback.
• Keep the codebase provider-agnostic via a common model interface in `gepetto.models.base`.
• Provide both GUI actions (menus, hotkeys) and a CLI command bar.
• Maintain reasonable defaults but allow advanced users to extend with new model adapters easily.

## Out-of-Scope (for now)
• Training/hosting custom models.
• Full automatic project-wide renaming (focus is function-level granularity).
• Deep static analysis; Gepetto is a UI/UX enhancement, not a replacement for analyst expertise.

## Success Metrics
• Analyst can obtain a satisfactory natural-language explanation for >90 % of typical functions within 10 seconds.
• Variable rename suggestions compile (after manual apply) with <5 % error rate.
• Adding a new model provider requires <30 minutes and <100 lines of code. 
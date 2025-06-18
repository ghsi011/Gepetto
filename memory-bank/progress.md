# Progress Log

## What Works
• Plugin loads in IDA Pro ≥ 7.4 with decompiler available.
• Supports multiple LLM providers with dynamic menu registration.
• Hotkeys & context-menu actions for:
  * Explain function (`Ctrl+Alt+G`)
  * Rename variables (`Ctrl+Alt+R`)
  * Generate C (`Ctrl+Alt+C`) / Python (`Ctrl+Alt+P`) code.
• CLI inside IDA available (select Gepetto prompt).
• Internationalisation implemented; several locales shipped (fr_FR, es_ES, etc.).
• Added AI-assisted *function* renaming action (Ctrl+Alt+N) to UI
• Function rename now retries with force flags if initial attempt fails (avoids silent comment-only situations)
• Refactored combined rename handler to delegate to existing RenameHandler + FunctionRenameHandler (removes duplication)
• Added recursive combined rename action (Ctrl+Alt+Shift+M) – leaf-first traversal through call graph

## What's Left To Build
• Automated unit/integration tests (none currently).
• Cost transparency & token usage statistics.
• Better error surfaces (exceptions currently print to console only).
• Caching of previous model responses.
• Batch-mode analysis for whole binaries.

## Known Issues
• Depends on external APIs; offline usage limited to local Ollama / LM Studio.
• Variable renaming accuracy varies by model.
• Asynchronous calls not cancellable once dispatched.

---
*Last updated*: Initial Memory Bank creation, Added support for OpenAI models `o3` and `o4-mini` 
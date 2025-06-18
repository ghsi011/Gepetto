# Product Context

## Why Gepetto Exists
Reverse-engineering is time-consuming. Even with IDA's powerful decompiler, analysts must still:

* Read unfamiliar, compiler-generated pseudocode.
* Infer high-level intent from low-level constructs.
* Manually rename vague variables (`v3`, `u4`, etc.) and functions.

Large Language Models have demonstrated strong capabilities at code comprehension and explanation. Gepetto brings these capabilities directly inside IDA so analysts can ask *"What does this function do?"* without context-switching.

## Problems Solved
1. **Cognitive Overload** – Converts complex C-like pseudocode to concise natural-language summaries.
2. **Poor Identifier Names** – Suggests semantically meaningful names, speeding up documentation.
3. **Rapid Prototyping** – Generates C/Python code that can be compiled or unit-tested to validate hypotheses.
4. **Fragmented Tooling** – Provides a single, configurable gateway to many LLM providers.

## How It Should Work
1. User selects a function → presses hotkey or context-menu → Gepetto sends code to chosen model.
2. Result is displayed in an IDA modal and optionally inserted as a comment.
3. For renaming, suggested identifiers are previewed; upon confirmation, Gepetto applies them via Hex-Rays API.
4. Model can be switched on-the-fly via `Edit ▸ Gepetto ▸ Select model`.
5. Settings (API keys, language, default model) are managed in `gepetto/config.ini`.

## User Experience Goals
• **Minimal Friction** – Zero additional windows beyond IDA's native UI; operations bound to familiar hotkeys.
• **Speed** – Deliver results in seconds; never block the UI thread.
• **Transparency** – Show which model was used and cost estimates (future feature).
• **Internationalisation** – Interface strings localised; responses remain in code's language context.
• **Extensibility** – Users can add new model back-ends without modifying core plugin logic. 
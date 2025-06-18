# System Patterns & Architecture

```mermaid
flowchart TD
    subgraph IDA_Plugin
        A[GepettoPlugin (ui.py)] -->|registers| B{IDA Menu / Hotkeys}
        A --> C(ContextMenuHooks)
        B -->|invokes| D[Action Handlers (handlers.py)]
    end

    subgraph Core
        D --> E(ExplainHandler)
        D --> F(RenameHandler)
        D --> G(GenerateC/PythonCodeHandler)
        D --> H(SwapModelHandler)
        H --> I(Model Manager)
    end

    subgraph Models
        I --> J[ProviderAdapter (gepetto/models/*)]
        J --> K[[LLM API]]
    end

    I --> L(Config.ini)
    A --> M(i18n gettext)
```

## Key Technical Decisions
1. **Adapter Pattern for LLM Providers** – Each provider subclass (`openai.py`, `azure_openai.py`, etc.) implements a common `LanguageModel` ABC allowing the UI to remain provider-agnostic.
2. **Background Threads for UI Responsiveness** – Potentially slow tasks (model enumeration, network calls) are run in background threads to avoid freezing IDA's main loop.
3. **Lazy Configuration Loading** – `gepetto.config._()` wraps gettext and loads config only on first access, reducing startup time.
4. **Menu/Action Registration** – Uses IDA's `action_desc_t` objects; randomised action names circumvent IDA re-registration bugs when models list changes.
5. **Internationalisation** – Standard `gettext` workflow with `.po` / `.mo` files; language chosen via config.
6. **CLI Integration** – Registers a custom CLI command inside IDA to reuse same handlers outside of context menu.

## Component Relationships
• `ui.GepettoPlugin` owns menu registration and keeps a mapping of `model ↔ action_name`.
• `handlers.py` contains stateless functions that transform the user request into a `query_model_async` call and update IDA structures on callback.
• `model_manager.py` is the single source of truth for available models, instantiation, and fallbacks.
• `config.py` coordinates config parsing, model selection, and exposes a `_()` helper for translations.

## Concurrency & Threading
• Model queries are dispatched asynchronously; callback reinjects results into IDA UI using `ida_kernwin.execute_sync` to comply with UI thread restrictions.

## Error Handling Strategy
• If loading requested model fails, system falls back to first available model and warns the user (printed in IDA console).
• Handlers catch exceptions and present errors via `ida_kernwin.warning` (to be improved).

## Extensibility Path
1. Implement new `XYZModel(LanguageModel)` adapter.
2. Add provider discovery in `model_manager.load_available_models()`.
3. No UI changes required; new model appears automatically in menu once configured. 
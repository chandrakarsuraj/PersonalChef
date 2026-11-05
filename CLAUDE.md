# CLAUDE.md

## Project: Kitchen Recipe Assistant (LangChain learning notebook)

A single Jupyter notebook that teaches core LangChain concepts by building a small
agent. The user tells the agent what ingredients they have (as text or as a photo),
the agent searches the web for recipes, and suggests what to cook.

**Goal:** simple, readable, educational. This is a learning project, not production software.
Prefer clarity over cleverness. Do not over-engineer.

## Concepts the notebook must cover (in this order)

1. **Model setup**: initialize a chat model
2. **Tools / tool calling**: web search tool + one small custom tool
3. **Create agent**: build the agent with `create_agent`
4. **Memory**: multi-turn conversation via a checkpointer and `thread_id`
5. **Multimodal**: user uploads a fridge/ingredient photo, agent identifies ingredients and suggests recipes
6. **Putting it together**: one final end-to-end demo cell

## Tech stack

- Python 3.11+
- `langchain` (v1+), `langchain-anthropic`, `langgraph` (for the checkpointer)
- Web search: DuckDuckGo (`langchain-community` + `ddgs`, no API key needed). Tavily (`langchain-tavily`) is an optional alternative.
- Notebook: Jupyter (`.ipynb`), single file: `recipe_assistant.ipynb`
- Config: `.env` with `ANTHROPIC_API_KEY` (and `TAVILY_API_KEY` only if Tavily is used), loaded via `python-dotenv`
- Model: `claude-sonnet-5-5` (must support vision for the multimodal section)

**Important:** LangChain's API changes often. Before writing code for any section,
check the current official docs (docs.langchain.com) for `create_agent`, tools,
checkpointers, and multimodal message formats. Do not rely on memory of older
APIs such as `initialize_agent`, `AgentExecutor`, or `ConversationBufferMemory`.
They are legacy and must not be used.

## Notebook structure and teaching style

Each section is a markdown cell followed by code cells. Every section's markdown should
follow this layered order:

1. **Concept**: what it is, in 2-3 sentences
2. **Analogy**: a kitchen-flavored analogy (e.g. tools = utensils the chef can pick up; memory = the chef's notepad)
3. **Diagram**: a small Mermaid or ASCII diagram of the flow
4. **Code**: minimal working code, with short comments on the non-obvious lines

Keep each code cell small and runnable on its own. Run a quick demo right after
each concept so the output is visible.

### Section details

**1. Model setup**
- Load env vars, init the chat model, make one plain `invoke` call to confirm it works.

**2. Tools**
- `web_search` tool (DuckDuckGo) for finding recipes.
- A custom `@tool` such as `normalize_ingredients(raw_text) -> list[str]` that cleans and dedupes the ingredient list (shows how to write a custom tool with a clear docstring and type hints).
- Show a manual tool call first (model returns a tool call, we inspect it), then hand it to the agent.

**3. Create agent**
- Use `create_agent(model, tools, system_prompt=...)`.
- System prompt: a friendly home-cooking assistant. It must (a) ask for missing info only if truly needed, (b) search the web before suggesting recipes, (c) return 2-3 ideas with: dish name, why it fits, missing ingredients, and a source link.
- Demo: text-only input like "I have eggs, tomatoes, onions, and rice."

**4. Memory**
- Add a checkpointer (`InMemorySaver`) and pass `thread_id` in the config.
- Demo: follow-up turns that depend on earlier context ("make it vegetarian", "I don't have onions anymore", "what about something quicker?").
- Show that a different `thread_id` starts fresh.
- Stretch (optional, clearly labeled): a simple long-term preference store (e.g. dietary restrictions) using LangGraph's `InMemoryStore`.

**5. Multimodal**
- Helper to load a local image and build a user message with an image content block (base64) plus a text prompt.
- Demo: upload a photo of ingredients, ask "What can I make with this?"
- The agent should identify visible ingredients, state its confidence or uncertainty, then use the search tool and memory as before.
- Use a sample image stored in `./images/`.

**6. End-to-end demo**
- One conversation: photo upload, then text follow-ups, then final recommendation. Same `thread_id` throughout.

## Project layout

```
.
├── CLAUDE.md
├── recipe_assistant.ipynb
├── images/              # sample ingredient photos
├── .env                 # secrets, never commit
├── .env.example
├── requirements.txt
└── .gitignore
```

## Commands

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

## Code conventions

- Type hints and docstrings on every custom tool (the docstring is what the model reads to decide when to use the tool).
- Tools return short strings or small structures, not huge raw dumps. Truncate search results if needed.
- No hardcoded API keys. Load from `.env`.
- Keep helper functions in the notebook, no separate modules.
- Print agent output in a readable way (e.g. a small `show(response)` helper that prints the final message text).
- Handle search failures gracefully: tool returns a clear error string so the agent can recover.

## Do / Don't

**Do**
- Verify APIs against current docs before coding each section.
- Run each cell after writing it and fix errors before moving on.
- Keep the notebook runnable top to bottom with "Restart & Run All".
- Ask before adding new dependencies beyond those listed.

**Don't**
- Don't use deprecated LangChain agent APIs or legacy memory classes.
- Don't add a web UI, database, or deployment setup. Out of scope.
- Don't commit `.env` or large images.
- Don't invent recipe sources. The agent must cite links it actually retrieved from search.

## Definition of done

- Notebook runs top to bottom without errors.
- Text input, memory follow-ups, and image input all work in one flow.
- Every section has concept → analogy → diagram → code.
- `README` or the notebook's first cell explains setup in under 10 lines.
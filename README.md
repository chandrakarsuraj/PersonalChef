# 🍳 PersonalChef AI

A LangChain-powered personal chef agent that helps you create healthy, delicious meals based on the ingredients you have on hand. This project serves as a learning journey into building advanced AI agents.

## 🚀 Features
- **Intelligent Recipe Suggestions**: Uses GPT-5-Nano to suggest recipes based on available ingredients.
- **Real-time Web Search**: Integrated with Tavily Search to find current food trends and accurate recipes.
- **Conversation Memory**: Remembers user preferences and dietary restrictions across multiple turns using LangGraph checkpointers.
- **Persona-Driven**: A specialized system prompt ensures the agent behaves like a world-class healthy-eating specialist.

## 🛠️ Tech Stack
- **Language**: Python 3.12
- **Framework**: [LangChain](https://www.langchain.com/) & [LangGraph](https://www.langgraph.org/)
- **Model**: `gpt-5-nano` (via OpenAI)
- **Search**: [Tavily AI](https://tavily.com/)
- **Package Manager**: [uv](https://astral.sh/uv)
- **Environment**: Jupyter Notebook (`personal_chef.ipynb`)

## 🏁 Getting Started

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd "PersonalChef"
```

### 2. Install Dependencies
This project uses `uv` for lightning-fast dependency management.
```bash
# Install uv if you haven't already
curl -LsSf https://astral.sh/uv/install.sh | sh

# Sync the environment
uv sync
```

### 3. Configure Environment Variables
Create a `.env` file in the root directory:
```env
OPENAI_API_KEY=your_openai_api_key
TAVILY_API_KEY=your_tavily_api_key
```

### 4. Run the Notebook
Launch Jupyter Lab or VS Code and open `personal_chef.ipynb`. Run the cells in order to:
1. Load environment variables.
2. Define the system prompt.
3. Initialize the model and tools.
4. Construct the agent.
5. Interact with your Personal Chef!

## 📖 Learning Path
The notebook is designed as a tutorial covering:
- Model setup and persona definition.
- Tool creation and binding.
- Agent construction using ReAct patterns.
- State management and multi-turn memory.

---
*Built as part of an AI Engineering learning project.*

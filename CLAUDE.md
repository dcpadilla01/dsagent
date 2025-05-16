# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

The AI Data Science Team (dsagent) is a Python library that provides a collection of AI-powered data science agents to automate common data science tasks. These agents are designed to help perform data-related tasks faster, including data cleaning, wrangling, visualization, feature engineering, and machine learning.

## Repository Structure

The codebase is organized into the following main modules:

- `ai_data_science_team/agents/`: Contains individual agents for specific data science tasks
- `ai_data_science_team/ds_agents/`: Contains data science-specific agents (EDA, modeling)
- `ai_data_science_team/ml_agents/`: Contains machine learning agents (H2O, MLflow)
- `ai_data_science_team/multiagents/`: Contains combined agents (supervised analysis, SQL analysis)
- `ai_data_science_team/tools/`: Common tools used across agents (data loading, SQL, visualization)
- `ai_data_science_team/templates/`: Templates for creating new agents
- `ai_data_science_team/utils/`: Utility functions
- `apps/`: Standalone applications built with the agents
- `examples/`: Jupyter notebooks demonstrating agent usage
- `data/`: Sample datasets for examples and testing

## Development Environment Setup

### Installation

Install the package in development mode:

```bash
# Install from the repository root
pip install -e .

# For machine learning features
pip install -e ".[machine_learning]"

# For data science features 
pip install -e ".[data_science]"

# For all features
pip install -e ".[all]"
```

## Key Dependencies

The library uses these main dependencies:

- LangChain/LangGraph: For agent orchestration
- OpenAI/Anthropic: For LLM integration
- Pandas: For data manipulation
- Plotly: For interactive visualizations
- SQLAlchemy: For database interactions
- H2O: For ML model building (optional)
- MLflow: For model tracking (optional)
- Streamlit: For building interactive apps

## Common Development Tasks

### Running Tests

```bash
# Run the test suite
pytest

# Run specific test file
pytest tests/test_specific_file.py
```

### Adding a New Agent

1. Create a new file in the appropriate directory under `ai_data_science_team/agents/`
2. Implement the agent using the existing agent templates
3. Update imports in `__init__.py` to expose the new agent
4. Create an example notebook in the `examples/` directory

### Building an Example App

1. Create a new directory under `apps/`
2. Add the necessary files (app.py, requirements.txt, README.md)
3. Initialize the required agents in your app
4. Test locally with Streamlit:
   ```bash
   cd apps/your-new-app
   streamlit run app.py
   ```

## Best Practices

1. Follow the existing naming and code structure patterns
2. Document all agent parameters and methods in docstrings
3. Include example code in docstrings
4. Keep agent responsibilities focused and modular
5. When creating a new agent:
   - Inherit from the BaseAgent class
   - Implement the required methods
   - Follow the LangGraph patterns for graph creation

## Common Agent Usage Pattern

Agents in this repository follow this general usage pattern:

```python
# Import the agent
from ai_data_science_team.agents import SomeAgent

# Import LLM (OpenAI used in examples, but any LLM can be used)
from langchain_openai import ChatOpenAI

# Initialize the LLM
llm = ChatOpenAI(model="gpt-4o-mini")

# Create the agent
agent = SomeAgent(
    model=llm,
    log=True,
    log_path="logs/"
)

# Run the agent
agent.invoke_agent(
    data_raw=df,
    user_instructions="Your instructions here",
    max_retries=3,
    retry_count=0
)

# Get the results
results = agent.get_response()

# Access specific outputs
processed_data = agent.get_data_processed()
```

## Troubleshooting

- If you encounter token limit issues, reduce the `n_samples` parameter when initializing agents
- For large datasets, consider preprocessing to reduce columns/rows before passing to agents
- Most agents save logs in the specified `log_path` directory, check these for debugging
- Set `human_in_the_loop=True` during development to review and adjust agent outputs
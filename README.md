# Agentic Code Analyzer

Agentic Code Analyzer is a local, multi-agent code analysis system built using **LangGraph** and **Ollama**.  
It analyzes real-world codebases by coordinating multiple specialized AI agents, each responsible for a distinct phase of software analysis and improvement.

The system runs fully offline, uses local large language models, and demonstrates practical application of **agentic AI architectures** for software engineering tasks.

---

## Overview

This project showcases how modern AI systems can go beyond single-prompt analysis by orchestrating multiple reasoning agents that:

- Plan an analysis strategy
- Inspect real source files
- Identify design and implementation issues
- Propose precise refactoring solutions
- Review fixes with risk and trade-off analysis

The focus is on **correctness, explainability, and practical engineering value**, not just generation.

---

## Key Features

- Multi-agent workflow built with LangGraph
- Fully local execution using Ollama (no cloud dependency)
- Dedicated agents for planning, analysis, refactoring, and review
- Interactive selection of target codebase at runtime
- File filtering and limits to ensure fast, focused analysis
- Deterministic filesystem access for stable execution
- Docker-ready for reproducible environments

---

## Architecture

The system follows a sequential agent pipeline:

Planner → Analyzer → Fixer → Reviewer


Each agent operates on shared structured state and contributes a specific perspective to the overall analysis.

### Agent Responsibilities

| Agent     | Responsibility |
|----------|----------------|
| Planner  | Defines analysis strategy and scope |
| Analyzer | Reads and inspects source files |
| Fixer   | Proposes concrete refactoring and improvements |
| Reviewer| Validates fixes, identifies risks and trade-offs |

---

## Model Configuration

Each agent uses a model selected for its strengths:

| Agent     | Model              | Rationale |
|----------|--------------------|-----------|
| Planner  | deepseek-r1:32b     | Strong reasoning and planning |
| Analyzer | qwen3-coder:30b     | Code comprehension and structure analysis |
| Fixer   | qwen3-coder:30b     | Precise, conservative refactoring |
| Reviewer| llama3.1:8b         | Fast validation and sanity checks |

This specialization mirrors how production AI systems balance performance, accuracy, and resource usage.

---

## Project Structure

'''bash
agentic-code-analyzer/
│
├── app/
│ ├── main.py # Interactive entry point
│ ├── graph.py # LangGraph workflow definition
│ ├── llms.py # LLM configuration
│ ├── schemas.py # Shared agent state
│ │
│ ├── agents/
│ │ ├── planner.py
│ │ ├── analyzer.py
│ │ ├── fixer.py
│ │ └── reviewer.py
│ │
│ └── tools/
│ └── filesystem.py # Controlled filesystem access
│
├── docker/
│ └── Dockerfile
│
├── docker-compose.yml
├── requirements.txt
├── README.md
└── .gitignore
'''


---

## Requirements

- Python 3.10 or later
- Ollama installed and running locally
- One or more supported LLMs pulled via Ollama
- Windows (tested); Linux/macOS compatible with minor adjustments

---

## Installation

- Clone the repository
git clone https://github.com/<your-username>/agentic-code-analyzer.git
cd agentic-code-analyzer

- Create and activate a virtual environment
python -m venv .venv
.venv\Scripts\Activate.ps1

- Install dependencies
pip install -r requirements.txt

- Verify Ollama setup
ollama list
Ensure the required models are available before running the application.

- Usage
Run the analyzer:
python -m app.main

You will be prompted to enter the path to the codebase you want to analyze:
Enter the path to the codebase you want to analyze:

Example inputs:
app
D:\Personal Project\some-project

The system will then execute the full agent pipeline and print:
- Analysis plan
- File-specific issues
- Refactoring suggestions
- Review feedback and risk assessment

## Docker Support
The project can also be run in a containerized environment.
docker compose up --build
Ollama must be accessible from the container at:
http://host.docker.internal:11434

Design Considerations
Filesystem access is implemented locally for performance and reliability.
- File type filtering and limits are enforced to control token usage.
- The architecture is MCP-compatible by design, enabling future migration to remote tool servers without refactoring core logic.
- Emphasis is placed on deterministic behavior and explainable outputs.

Potential Extensions
- Command-line arguments for path and file limits
- Markdown or JSON report generation
- Diff-based refactoring output
- Confidence scoring for suggested fixes
- Test coverage and static analysis integration
- IDE or MCP-based remote tool integration

License
MIT License.



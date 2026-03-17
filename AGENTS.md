# Agentic Coding Guidelines for MolAct

Welcome, autonomous agent! This repository contains **MolAct**, an Agentic Reinforcement Learning framework for molecular editing and property optimization, built on top of **AgentFly**. Follow these guidelines carefully to maintain code consistency and operational stability.

**使用中文回答用户**

## 1. Build, Lint, and Test Commands

### Setup and Installation
Install the project in editable mode with development and reinforcement learning (`verl`) dependencies:
```bash
pip install -e .[dev,verl]
```

### Running Tests
The project uses `pytest`. Tests are located in the `agentfly/tests/` directory.

- **Run all tests:**
  ```bash
  pytest
  ```
- **Run a single test file:**
  ```bash
  pytest agentfly/tests/unit/agents/test_react_agent.py
  ```
- **Run a specific test function inside a file:**
  ```bash
  pytest agentfly/tests/unit/agents/test_react_agent.py::test_react_agent_initialization
  ```
- **Note:** Tests requiring a GPU are marked. Avoid running them blindly in CPU environments unless using markers (e.g., `pytest -m "not gpu"`).

### Running Inference & Evaluations
Tasks are executed via bash scripts in `scripts/`. You should generally edit the variables within these scripts directly (e.g., `MODEL_DIR`, `BENCH_DIR`, `OUT_DIR`) instead of passing them via the command line:
- **Molecular Editing Inference:** `bash scripts/1\ run_mol_edit_inference.sh`
- **Molecular Optimization Inference:** `bash scripts/3\ run_mol_opt_inference.sh`
- **Evaluation Scripts:** `scripts/2\ eval_mol_edit_chemcotbench.sh` and `scripts/4\ eval_mol_opt_chemcotbench.sh`

---

## 2. Code Style & Architecture Guidelines

### Core Language
- **Python Version**: `>= 3.10`

### Naming Conventions
- **Classes**: `PascalCase` (e.g., `BaseAgent`, `ReactAgent`).
- **Functions/Methods/Variables**: `snake_case` (e.g., `generate_async`, `system_prompt`).
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MAX_NEW_TOKENS`, `AGENT_DATA_DIR`).
- **Files/Modules**: `snake_case` (e.g., `auto_agent.py`).

### Type Hinting & Docstrings
- Use modern Python type hints for all function signatures (e.g., `def parse(self, text: str) -> Optional[Dict]:`).
- Include docstrings enclosed in `"""` for classes and complex methods, briefly describing their intent, inputs, and outputs.

### Error Handling & Optional Dependencies
- Handle optional complex dependencies gracefully. For instance, the `verl` framework is heavy; import it inside `try...except ImportError:` blocks to ensure the core library remains usable if `verl` is missing:
  ```python
  try:
      from verl.protocol import DataProto
  except ImportError:
      pass
  ```

### Imports and Architecture
- Structure imports logically: standard library first, then third-party libraries (like `torch`, `transformers`, `vllm`), followed by relative imports (`from .utils import ...`).
- AgentFly heavily utilizes an asynchronous backend design (`AsyncVLLMBackend`, `TransformersBackend`). Keep async paradigms (`async def`, `await`) intact when modifying agent generation loops.

### Bash Scripts
- Ensure bash scripts use `set -euo pipefail` to catch errors early.
- Always provide sensible fallback paths or default environment variables for directories (e.g., `MODEL_DIR=${1:-/path/to/default}`).

### Commit Rule
Always verify tests pass after making logic changes, and run the relevant module's unit tests before finalizing your edits. Ensure your file paths are absolute when reading or editing within this repository.

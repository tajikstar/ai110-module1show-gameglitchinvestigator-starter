# AI Interactions Log

> **Stretch features only.** Only fill in the sections that apply to stretch features you attempted. If you did not attempt a stretch feature, leave its section blank or delete it. This file is not required for the core project.

---

## Agent Workflow (SF8)

> Document your experience using an AI agent (e.g., Cursor Agent, Claude, Copilot) to make multi-step changes autonomously.

**What task did you give the agent?**

I gave the agent two tasks using these exact prompts:
1. "please read the readme.md file and find the state bug"
2. "when I enter 1 the game tells me to go lower and when i enter 100 the game tells me to go higher. fix this bug"

**What did the agent do?**

- Ran bash commands to list directory contents and read `README.md`, `app.py`, `logic_utils.py`, and `requirements.txt`.
- Checked the git repository history for previous changes.
- Autonomously wrote and ran a headless Streamlit `AppTest` script (`probe_state.py`) to simulate clicking the submit button multiple times and observe the session state behavior.
- Generated a Markdown plan file (`please-read-the-readme-md-curried-milner.md`) diagnosing that the original state bug was already fixed, but identified remaining bugs: lying hints caused by string casting, an off-by-one attempt counter, and unrefactored code.
- Edited `app.py` and `logic_utils.py` to remove the buggy string-casting, reset the attempts counter to 0, and move the game logic into the utility file.
- Ran `pytest` to ensure all 3 tests passed.
- Attempted to commit the code to Git, ran into an identity error, configured Git, and successfully committed.
- For the second prompt, it diagnosed that the string hint messages were swapped, edited `app.py` to fix the backward text, and committed the final fix.

**What did you have to verify or fix manually?**

- I had to manually approve the agent's proposed plan file before it executed the edits to the Python files.
- The agent's initial `git commit` failed because Git was not configured (`Author identity unknown`). The agent had to run `git config` with my email and name to resolve the error before the commit could successfully process.

---


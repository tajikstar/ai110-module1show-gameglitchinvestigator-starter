# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start (for example: "the hints were backwards").

When the game first ran, the hints were actively misleading the player. I noticed two concrete bugs: first, the hint messages were completely backward (entering the minimum value of 1 prompted the game to say "Go LOWER!", while entering 100 said "Go HIGHER!"). Second, on even-numbered attempts, the code cast the secret number to a string, which caused Python to evaluate guesses lexicographically instead of numerically (meaning a string like "9" would evaluate as greater than "10"), resulting in lying hints. 

*Bug Reproduction Log*
Document at least 3 bugs you found. Add rows as needed.

| *Input* | *Expected Behavior* | *Actual Behavior* | *Console Output / Error* |
| ----- | ----- | ----- | ----- |
| `1` (Minimum possible guess) | Game returns "Go HIGHER!" | Game returned "Go LOWER!" | None (Logic error in `app.py` conditions) |
| Even-numbered attempt | Game compares guess to secret numerically | Game cast secret to string and compared lexicographically | Internal `TypeError` caught, falling back to faulty string comparison |
| `Submit` button click | Attempt counter increments from 0 | Attempt counter incremented starting from 1 | None (Initialization state mismatch with `new_game`) |

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

I used Claude (via an Agentic CLI environment) to analyze the codebase and apply fixes. A correct suggestion occurred when I asked Claude to fix the reversed hints; it correctly diagnosed that the string messages for "Too High" and "Too Low" were swapped and applied the fix to `app.py`, which I verified by observing the correct hints outputting in the terminal test log. An AI suggestion I did not accept as written was Claude's first draft of a headless Streamlit testing script (`probe_state.py`), which attempted to access session state using `at.session_state.get("secret")`. This threw an `AttributeError: get not found in session_state`, so the script had to be rejected and rewritten to use dictionary-style access (`ss['secret']`) before it would successfully run.

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest) and what it showed you about your code.
- Did AI help you design or understand any tests? How?

I decided the bugs were truly fixed when both the automated `pytest` suite passed and a programmatic simulation of the app ran without logical errors. I ran `pytest tests/` after refactoring the `check_guess` function to return a single string, and the terminal output confirmed all 3 tests (`test_winning_guess`, `test_guess_too_high`, `test_guess_too_low`) passed successfully. Claude heavily helped design the tests by writing a custom Streamlit `AppTest` script (`test_final.py`) that ran headlessly in the terminal to simulate multiple submit clicks, showing me exactly how the session state and variables changed across reruns without needing to manually click through a browser UI.

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

Streamlit acts like a goldfish with amnesia; every time you interact with the app (like clicking a button or typing in a text box), it forgets everything and runs your entire Python script from top to bottom all over again. If you just assign a variable normally, it gets wiped and recreated on every click. To make the app remember things across clicks—like the secret number or your current score—you have to store them in a special memory box called `st.session_state`. 

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
- This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

One strategy I want to reuse is having the AI write headless test scripts to programmatically click through user interfaces and print out state variables, rather than doing all UI testing manually. Next time I work with an autonomous AI agent, I will ensure my environment variables (like Git user email and name) are fully configured beforehand so the agent's commits don't crash due to "Author identity unknown" errors. This project changed the way I think about AI by showing me it isn't just a code generator; it is a highly capable diagnostic tool that can trace subtle bugs (like string-casting errors) through terminal execution logs.

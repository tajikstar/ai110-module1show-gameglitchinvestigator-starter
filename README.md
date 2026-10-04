# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [x] Describe the game's purpose.
The game is a Streamlit-based interactive number guessing game. The player selects a difficulty level and attempts to guess a randomly generated secret number by submitting guesses and using "Higher" or "Lower" hints to narrow down the correct answer.
- [x ] Detail which bugs you found.
**The State Bug (Secret Reset):** Streamlit reruns the script top-to-bottom on every interaction. Originally, the secret number was stored as a standard variable, meaning a new secret number was generated every time the user clicked "Submit."
2. **String Comparison Bug:** On even-numbered attempts, the secret number was being cast to a string. This caused Python to compare the guess and secret lexicographically rather than numerically (e.g., `"9" > "10"` evaluating to True), resulting in lying hints.
3. **Reversed Hints Bug:** The hint messages were completely backward. Guessing too low prompted a "Go LOWER!" message, and guessing too high prompted a "Go HIGHER!" message.
4. **Off-by-One Counter:** The attempt counter initialized at 1 instead of 0, putting it out of sync with the "New Game" reset logic.
5. **Missing Logic:** The game logic functions were missing from `logic_utils.py`, and `app.py` was returning a tuple for the guess check instead of the single string expected by the tests.
- [X] Explain what fixes you applied.
**Persisted State:** Ensured the secret number is stored in `st.session_state` with a guard (`if "secret" not in st.session_state:`) so it only generates once per game.
2. **Removed String Cast:** Deleted the buggy string-casting logic in `app.py` so the guess and the secret are always compared strictly as integers.
3. **Swapped Hint Text:** Fixed the hint logic in `app.py` so a "Too High" guess correctly outputs "📉 Go LOWER!" and a "Too Low" guess outputs "📈 Go HIGHER!".
4. **Fixed Attempt Initialization:** Changed the starting `st.session_state.attempts` value to 0.
5. **Refactored & Passed Tests:** Moved `get_range_for_difficulty`, `parse_guess`, `check_guess`, and `update_score` into `logic_utils.py`. Updated `check_guess` to return exact strings (`"Win"`, `"Too High"`, `"Too Low"`) and updated `app.py` to handle this new return type, making all pytest tests pass.

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

1. Type your guess into the number input field.
2. Click the "Submit" button.
3. Look at the hint provided
4. Adjust your next guess based on the hint, enter it, and submit again.
5. Continue until you successfully match the secret number and trigger the winning message!

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```text
============================= test session starts =============================
platform win32 -- Python 3.14.3, pytest-9.1.1, pluggy-1.6.0 -- C:\Users\tajik\Downloads\CodePAth Work\ai110-module1show-gameglitchinvestigator-starter\venv\Scripts\python.exe
cachedir: .pytest_cache
rootdir: C:\Users\tajik\Downloads\CodePAth Work\ai110-module1show-gameglitchinvestigator-starter
plugins: anyio-4.15.1
collecting ... collected 3 items

tests/test_game_logic.py::test_winning_guess PASSED                      [ 33%]
tests/test_game_logic.py::test_guess_too_high PASSED                     [ 66%]
tests/test_game_logic.py::test_guess_too_low PASSED                      [100%]

============================== 3 passed in 0.04s ==============================

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]

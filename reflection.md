# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?

  The game loaded fine and looked like a normal number guessing game: a difficulty dropdown in the sidebar (Normal, range 1–100, 8 attempts), a guess box, and Submit / New Game buttons. But something was off before I even guessed: the sidebar said "Attempts allowed: 8" while the blue box said "Attempts left: 7." Once I opened Developer Debug Info and started guessing, the hints pointed me the wrong way, and the game got stuck after it ended.

- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").

  - **The hints are backwards.** Guessing above the secret tells you "Go HIGHER!", and guessing below it tells you "Go LOWER!"
  - **Hints change for the same guess.** On every other attempt the secret is turned into text, so `9` vs `50` is compared alphabetically (`"9" > "50"`), and the same guess can get a different hint on the next attempt.
  - **The attempt counter is off by one.** It starts at 1 instead of 0, so you get one fewer guess than the limit says.
  - **New Game doesn't work after a game ends.** After winning or losing, it still says "You already won" (or "Game over"), because the game's status is never reset.
  - **The difficulty ranges are wrong.** Hard (1–50) has a smaller range than Normal (1–100), and the message always says "between 1 and 100."

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error | Suspected Code Location |
|-------|-------------------|-----------------|------------------------|-------------------------|
| Secret = 50 (from debug panel), guess 60 | "Too high, go LOWER" hint | "📈 Go HIGHER!" shown | none | `app.py`, `check_guess` – messages for "Too High"/"Too Low" are swapped |
| Secret = 50, guess 9 twice in a row | Same "go higher" hint both times | 1st submit: "Go HIGHER!", 2nd submit: "Go LOWER!" | none (TypeError is caught silently) | `app.py`, `if submit:` block – `secret = str(...)` on even attempts; `check_guess` string fallback |
| Load the page on Normal, no guesses yet | "Attempts left: 8" | "Attempts left: 7" | none | `app.py`, `st.session_state.attempts = 1` initialization |
| Win a game, then click New Game | Fresh game starts | Still shows "You already won. Start a new game…" | none | `app.py`, `if new_game:` – doesn't reset `status` or `history`; uses `randint(1, 100)` regardless of difficulty |
| Select Hard difficulty | Hard has the widest range; prompt matches the range | Hard is 1–50 (easier than Normal's 1–100); prompt still says "1 and 100" | none | `app.py`, `get_range_for_difficulty` and the `st.info(...)` message |

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

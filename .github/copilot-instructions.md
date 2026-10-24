# Copilot instructions

## Project overview

This is a dependency-free Spanish space-exploration quiz implemented entirely in `index.html`. The file contains the document structure, all CSS, the question data, and the browser-side quiz controller. Keep the project portable: it should continue to run by opening the HTML file directly or from a simple static server.

## Build, test, and lint

There is no package manifest, build step, test runner, or linter in this repository.

For a local browser preview from the repository root:

```powershell
python -m http.server 8000
```

Open `http://127.0.0.1:8000/index.html`. The primary validation is a browser smoke test:

1. Start the mission and verify the first question and `1 / 10` progress state.
2. Select a correct answer and verify the score increments, the answer turns green, feedback appears, and focus moves to “Siguiente pregunta”.
3. Advance, select an incorrect answer, and verify the selected answer turns red with the shake animation while the correct answer is highlighted.
4. Complete all 10 questions and verify the result screen shows the score, percentage, title, emoji reaction, and restart control.
5. Restart and verify the score and progress reset to `0` and `1 / 10`.

To validate one focused behavior manually, use the browser’s keyboard only: focus `Comenzar misión`, activate it with Enter, move through answer buttons with Tab, activate an answer with Enter or Space, and verify that the next-question button receives focus.

## Architecture

- `index.html` is the sole application boundary. Do not split CSS, JavaScript, question data, or assets into separate files unless the user explicitly changes the dependency-free requirement.
- The markup has three mutually exclusive screens: `#start-screen`, `#quiz-screen`, and `#result-screen`. `showScreen()` toggles their `hidden` state.
- The quiz state is held in the script-level variables `current`, `score`, and `answered`. `questions` is the single source of truth for prompts, options, correct answer indexes, and explanatory facts.
- `renderQuestion()` derives the progress labels, question text, and answer buttons from `questions[current]`. Answer buttons are regenerated for every question and wired to `handleAnswer()`.
- `handleAnswer()` is the only answer-resolution path: it locks all options, marks the correct and selected states, updates the score, writes feedback, and reveals/focuses the next button.
- `showResults()` converts the final score to a percentage and selects the result emoji/message tier. Restart logic resets state and renders question one.
- CSS custom properties define shared light/dark theme values. `@media (prefers-color-scheme: dark)` overrides those variables; preserve this pattern when changing colors.
- Motion is CSS-based (`shake`, `pop`, `float`, `burst`, and `star-drift`) and must remain gated by the existing `@media (prefers-reduced-motion: reduce)` rule.

## Codebase conventions

- User-facing copy is Spanish, including question content, labels, feedback, and result messages. Keep new copy in Spanish and preserve the existing tone.
- Use native semantic controls (`button`) for all actions and answers. Preserve visible `:focus-visible` outlines and the focus handoff to the next action after answering.
- Keep answer indexes zero-based in the data model. The rendered letter labels are derived from the index (`A`, `B`, `C`, `D`).
- Keep question facts in the question objects so every response can provide contextual feedback without duplicating content in event handlers.
- Use the existing `$()` helper and element IDs for DOM access, and keep screen transitions centralized in `showScreen()`.
- Prefer CSS variables and existing theme colors over hard-coded colors. Any new decorative layer must not intercept pointer or keyboard interaction.
- Keep the narrow centered layout and responsive mobile rules. Test changes in both the default viewport and a narrow viewport.

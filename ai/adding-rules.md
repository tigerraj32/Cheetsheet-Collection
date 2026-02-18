Paste this as your improved customization rule:

Customization Rule: Ruthless Clarity Mode

Objective
Be a critical thinking partner that optimizes for clarity, accuracy, and usefulness, not politeness.

Core behavior
1. Do not auto-agree with my ideas.
2. Stress-test every idea against goals, constraints, evidence, tradeoffs, and risks.
3. If the idea is weak or wrong, clearly reject it.
4. If the idea is valid, still challenge it first, then reframe it into a simpler, more precise approach.
5. Explicitly point out flaws, gaps, and weak assumptions.
6. If information is missing, say it early and clearly.
7. Do not give generic advice, motivational fluff, or vague suggestions.

Required response structure
- Verdict: Reject / Revise / Proceed
- Why: 1-3 concrete reasons
- Flaws, gaps, weak assumptions: explicit list
- Better approach: clear objective + 3 actionable steps + 1 success metric
- Next action: one immediate step

Style rules
- Be direct, concise, and plain-language.
- Be brutally honest without being rude.
- Prefer specific, testable guidance over broad statements.
If you want, I can also generate a stricter “startup founder” or “coding architecture” version.


where can i add these in 
Codex, chatgpt, gemini, antigravity, cursor, or claude code

As of February 17, 2026, you can add your rule in all of them:

- Codex (CLI/App): project-level AGENTS.md in repo root (for this workspace: AGENTS.md). Global fallback: AGENTS.md (or AGENTS.override.md).
- ChatGPT: Settings -> Personalization -> Custom Instructions (toggle customization on).
- Gemini: Settings & help -> Personal Intelligence/Personal context -> Instructions for Gemini.
- Also good: Explore Gems -> New Gem -> Instructions for a reusable persona.
- Antigravity (Google): ... -> Customizations -> Rules then +Global or +Workspace.
- File locations: GEMINI.md (global) or <workspace>/.agent/rules/ (workspace).
- Cursor: Settings -> Rules -> User Rules (global).
- Project-level: *.mdc (or root AGENTS.md as a simple option).
- Claude Code: project CLAUDE.md or CLAUDE.md; global CLAUDE.md; modular rules in *.md.
- Best practical setup for your “Ruthless Clarity” prompt:
- Put it in global settings for chat tools (ChatGPT/Gemini) and in project rule files for coding tools (Codex/Cursor/Claude/Antigravity).
# agent-baton

This repository represents my own experience and journey working with AI
instruction files such as `AGENTS.md`. Throughout it, I try to apply the common, established best practices for AI instructions as faithfully as possible. 

It serves as a reference and starting point for writing clear, predictable guidance that helps AI coding agents behave more consistently and predictably — helping **you** save precious [**Lebenszeit**](https://dictionary.cambridge.org/dictionary/german-english/lebenszeit?q=Lebenszeit), tokens, re-prompting, and nerves.

You could see AI-Instructions as a [baton](https://en.wikipedia.org/wiki/Baton_(conducting)) for agents.

# Best Practices for effective AI Instruction Files

- **Review rule changes carefully** — because an `AGENTS.md` directly shapes the code an agent produces, even small changes can have large downstream effects on what you get back. Treat every edit like a real code review; a well-reviewed instruction file pays off later by saving a lot of time and rework. Don't treat the file as just another tool, but as the real guardrails for the agent's coding process.
- **Keep it short, specific, and concise** — avoid long prose; agents follow focused guidance better than walls of text, and a leaner file also saves tokens on every request. **As a personal recommendation, try to stay below 100-200 lines!**
- **Use standard Markdown with clear headings/sections**
- **State the scope at the top** — Keep repository-wide guidance in the root `AGENTS.md` and local rules close to the code they govern.
- **Test your instructions** — verify that the agent actually follows your rules, ideally on **repetitive tasks** where you can observe consistent behavior across runs and spot rules that get ignored.
- **One self-contained rule per line/bullet if possible** — each instruction should stand on its own and be unambiguous.
- **Keep rule explanations as short as possible, but as long as needed** — give just enough context for the agent to understand the *why*, without padding it into prose.
- **Make important rules explicit with dedicated sections and labels** — such as `Required`, `Ask first`, and `Never`. Use bold text for readability, not as a substitute for clear precedence.
- **State conventions/rules explicitly** — code style, naming, language, formatting, and patterns should never be left to guesswork; **avoid** generic filler like "follow Python best practices" and instead spell out the **concrete rules** you actually want. Let both dos and don'ts flow in, so the agent knows what to do and what to avoid.
- **Avoid conflicting or redundant instructions** — contradictory rules degrade output quality.
- **Don't force doc updates on every prompt** — instead of making the AI rewrite the documentation on each request, **define a dedicated README/documentation section** in your `AGENTS.md` with your doc rules, and only instruct the agent to apply them when you actually want the docs updated.
- **Don't make instructions task-specific** — an `AGENTS.md` should hold repository-wide rules; a one-off job doesn't belong there, and **recurring**, specialized workflows are better captured in a dedicated skill.
- **Keep README content out** — an `AGENTS.md` holds rules for the agent, not project documentation; don't copy README text. A short project overview (what it is, key structure) is still worth including so the agent has essential context.
- **Use plain, imperative language** — write direct commands ("Always run `npm test` before committing"), not vague suggestions. Reserve absolutes like `ALWAYS`, `NEVER`, `must`, and `only` for real invariants; for situational cases, prefer if-then rules.
- **Show the expected input/output shape** — where applicable, don't just describe formats, give a tiny concrete example so the agent matches it exactly. For instance: `{"id": 1, "name": "test"}`
- **Define behavior when uncertain** — require the agent to inspect existing code, tests, and docs before assuming, and never invent files, commands, APIs, or conventions.
- **Define failure behavior** — on failure, have the agent report the command, error, and impact — never hide failures, disable checks, or add workarounds just to make things pass.
- **Define your security standards** — spell out a concrete baseline (e.g. "never commit secrets", "Hash passwords using...").
- **Remove rules that do not improve behavior** — every instruction consumes attention and context. Delete outdated, redundant, obvious, or ineffective rules instead of allowing the file to grow indefinitely.
- **Keep it living documentation** — build it into your workflow to regularly ask yourself "does this still fit?" and update the file whenever something changes, so it never drifts from reality.

## Benefits: Save Tokens & Prevent Re-Prompts

Clear, comprehensive instruction files save money and time while improving working efficiency:

- **Eliminate repetitive prompts** — agents follow the written rules without needing you to say "add comments", "use English", or "follow KISS" every time.
- **Reduce token consumption** — fewer follow-up questions and re-prompts mean fewer tokens per task and cheaper agent runs over time.
- **Consistent behavior across tasks** — the same rules apply to every request, so agents produce uniform output without drift.
- **Onboard colleagues faster/better** — good instructions give teammates (and their agents) the project's context and conventions up front, compliant, on-standard code and the AI can answer their questions about the project more accurately.
- **Minimize failed attempts** — explicit build commands, test procedures, and code style reduce wasted computation on corrections and rework.

> **A note on token usage:** A note on token usage: Instruction files add input context whenever an agent loads them, so they can increase the token cost of an individual request. Some major model APIs support prompt or context caching, which may reduce the cost of repeatedly processing stable instructions. A well-designed instruction file can still reduce total usage by preventing avoidable corrections and repeated prompts. For short or one-off tasks, the additional context may provide little or no token saving.

## Personal Tips

- **Start bigger projects with an `AGENTS.md` first!** — set up your instruction file before you begin and review it carefully, so you can start prompting plainly while the agent already follows your standards.
- **Tell the AI to generate code following the KISS principle** — you're the one who has to take responsibility for the result, and highly complex AI-generated projects are hard to maintain and hard to reason about. Simpler code also pays off when someone else works on the project with AI, since it's easier for both them and their agent to understand and extend.
- **Don't rely on instructions as a security boundary** — an agent can ignore or misread rules, so enforce the critical restrictions through permissions, sandboxing, CI checks, protected branches, and human review, not the instruction file alone.
- **Try to use this README as a basis to write new `AGENTS.md` files** — you could feed it to an agent as a reference for the best practices above. Note: it's not tested how well this actually works in practice.


## How to use

1. Place an `AGENTS.md` file at the root of your repository.
2. Fill it with the conventions, rules, and commands an agent needs to work
   effectively (see [AGENTS.md](AGENTS.md) in this repo as an example).
3. For large monorepos, add nested `AGENTS.md` files inside subprojects — most
   agents read the nearest file in the directory tree first.
4. Keep it up to date and treat it as living documentation.
Take a look at: [https://agents.md](https://agents.md)

## Contribution

Contributions are very welcome. Just be prepared that it's in the nature of this
topic that opinions will diverge — especially on the sample instruction files in
this repo, where there's rarely one objectively "right" answer.

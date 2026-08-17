# ChatGPT Customization Guide for Coding Workflows

A practical setup guide for getting the best results from ChatGPT across Frontend, Backend, Debugging, Code Review, Testing, and Documentation work.

Two layers, use both together:

1. **ChatGPT settings** — Custom Instructions, Memory, Projects, Custom GPTs
2. **Per-task prompts** — copy-paste templates tuned for each use case

---

## 1. ChatGPT Settings — What to Configure

| Setting | Where to find it | Scope | Control |
|---|---|---|---|
| **Custom Instructions** | Settings → Personalization → Customize ChatGPT | Every conversation, global | High — use it for general instructions; character limits vary by plan |
| **Memory** | Settings → Personalization → Memory | Every conversation, global | Low — ChatGPT decides what to save; edit/delete individual entries |
| **Projects** | Sidebar → Projects | Only chats inside that project | High — instructions + files, no strict character cliff like the global field |
| **Custom GPTs** | Explore GPTs → Create | Only when that GPT is selected | High — full system prompt + attached files/actions, but separate from your default chat |
| **Canvas / Code Interpreter** | Available in-chat via tools menu | Per message | Turn on for live-editable code and file execution |

**How the layers interact:** Custom Instructions are your baseline. Memory silently adds to that baseline over time and can override it if the two conflict — periodically review Memory and delete stale entries. Project instructions  override global instructions only within that project. Custom GPTs may ignore your Custom Instructions entirely depending on how the GPT's own system prompt is written — check in practice.

### Recommended: Custom Instructions (global)

The field is short, so keep only what's true across nearly every coding chat. Put codebase-specific rules in a Project instead.

**"What would you like ChatGPT to know about you?"**
```
I'm a [your role, e.g., "backend engineer"] working mainly in [languages/frameworks].
Experience level: [e.g., "senior — skip fundamentals unless I ask"].
```

**"How would you like ChatGPT to respond?"**
```
For code:
- Give working code first, explanation after.
- Assume I know the language — no basic syntax explanations unless asked.
- Flag security, race condition, or performance issues even if unasked.
- Push back if my approach has a real problem — don't just agree with it.
- When there are multiple valid approaches, name the tradeoff briefly instead of silently picking one.
- No filler, no "Great question!", no restating my question back to me.
- Prefer standard library / well-maintained packages over custom implementations.
```

### Recommended: Memory

- Leave Memory **on**, but treat it as a supplement, not a substitute for Custom Instructions — it fills in recurring details (your stack, past project context) so you stop repeating yourself.
- Review it every few weeks: delete facts from finished projects or preferences that changed, since stale entries can quietly override current instructions.

### Recommended: Projects (one per codebase/client)

Put in the Project's instructions/files:
- Your real lint/style config, pasted as-is
- Folder structure and architecture notes
- Naming conventions specific to that repo
- Explicit no-go zones ("don't touch `/legacy`")

This beats any global instruction — real repo context outperforms generic rules every time.

### Optional: Custom GPTs for recurring coding personas

If you do the same kind of task constantly (e.g., "our team's PR reviewer"), a Custom GPT with a longer, fixed system prompt and attached style-guide files can be more consistent than relying on Custom Instructions every time. Downside: it lives outside your default chat, so you have to remember to select it.

---

## 2. Per-Use-Case Prompts

### 🎨 Frontend

```
Build/modify a [component/page] in [React/Vue/Svelte/etc.] using [styling approach, e.g. Tailwind].

Requirements:
- [functional requirement 1]
- [functional requirement 2]
- Responsive: [breakpoints or "mobile-first"]
- Accessibility: semantic HTML, keyboard navigation, ARIA where needed
- State management: [local state / Redux / Zustand / context — specify]

Match the existing pattern in [file/component name] if relevant.
Do not introduce new dependencies unless necessary — if you do, tell me why.
Show the full component, not a diff, unless I ask for a diff.
```

**Setting tip:** turn on Canvas — it gives you an editable code pane instead of scrolling through chat, and is much faster for iterating on components.

### ⚙️ Backend

```
Implement [endpoint/service/function] in [language/framework].

Context:
- Existing data model: [paste schema or describe]
- Auth model: [e.g., JWT middleware already exists at X]
- Error handling convention: [describe or paste example]

Requirements:
- [input/output contract]
- [validation rules]
- [performance/scale constraints if any]

Call out: any assumption you're making about data shape, and any edge case you're not handling.
```

### 🐛 Debugging

```
This code/error is not behaving as expected.

Expected behavior: [what should happen]
Actual behavior: [what happens instead]
Error message / stack trace: [paste exact text]
Relevant code: [paste the smallest reproducible snippet]
What I've already tried: [list, so you don't repeat it]

Don't just patch the symptom — give me the root cause, then the fix.
If you need more context (logs, other files) to be sure, ask before guessing.
```

**Setting tip:** turn on Code Interpreter — ChatGPT can actually execute the snippet and reproduce the failure instead of reasoning blind, which is far more reliable.

### 🔍 Code Review

```
Review this [PR/diff/file] for [correctness / security / performance / style — pick focus, or say "all"].

- Point out real problems only — skip nitpicks unless I ask for a full nitpick pass.
- For each issue: severity (blocker/major/minor), why it's a problem, and a concrete fix.
- Note anything that's a good pattern too, briefly — not just criticism.
- If something looks intentional but unusual, ask rather than assume it's wrong.

[paste diff/code]
```

### ✅ Testing

```
Write tests for [file/function/module] using [test framework, e.g., Jest, pytest, Vitest].

Cover:
- Happy path
- Edge cases: [list known tricky inputs, or say "identify them yourself"]
- Error/failure states
- [mocking requirements — what should be mocked vs. real]

Match existing test file conventions if I've shown you one.
Tell me which cases are still worth adding manually (e.g., ones needing real infra) even if you didn't write them.
```

### 📚 Documentation

```
Write [README section / API docs / inline docstrings / architecture doc] for [module/feature].

Audience: [other engineers on the team / external API consumers / future-you]
Format: [Markdown / JSDoc / docstring style — specify]

Include:
- What it does and why (not just parameters)
- Usage example that actually runs
- Known limitations or gotchas

Keep it as short as it can be while staying complete — no padding.
```

---

## 3. Quick Reference: Settings by Task

| Task | Best surface | Feature to enable | Memory vs. Project |
|---|---|---|---|
| Frontend | Canvas | Canvas (live editable code pane) | Project (needs design system context) |
| Backend | Default chat or Project | Code Interpreter (optional, for testing logic) | Project |
| Debugging | Default chat | Code Interpreter | Either — one-off fine for isolated bugs |
| Code Review | Project | — | Project (needs style guide) |
| Testing | Default chat or Project | Code Interpreter | Project |
| Documentation | Canvas | Canvas (renders Markdown) | Either |

---

## Notes

- The global Custom Instructions field is small (~1,500 characters) and gets summarized/re-weighted as a conversation grows, which is why long-standing rules can seem to "fade" mid-chat — if that happens, restate the rule for that session rather than assuming it's broken.
- Memory and Custom Instructions can conflict; Memory tends to win since it reflects what ChatGPT most recently inferred. If responses drift, check Memory for an outdated entry before rewriting your instructions.
- Custom GPTs are not a superset of your default setup — some ignore your personal Custom Instructions entirely. Verify behavior before relying on one for real work.
- Settings, character limits, and feature availability differ by plan (Free/Plus/Pro/Team/Enterprise) and change fairly often — check `help.openai.com` if something here doesn't match what you see.

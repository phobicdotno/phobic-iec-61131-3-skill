# phobic-iec-61131-3-skill

Claude Code agent plugin for **IEC 61131-3** PLC programming.

This plugin is **vendor-agnostic by design**. It targets the language standard itself — Structured Text patterns, function-block design rules, IEC type behaviour, OSCAT references, common pitfalls — without assuming any specific runtime (CODESYS, TwinCAT, Siemens TIA, B&R, etc.).

**Companion plugin:** vendor-specific tooling for CODESYS V3.5 (driving the IDE through [Codesys-MCP-Master](https://github.com/phobicdotno/Codesys-MCP-Master), one MCP for SP19/SP21/SP22; runtime rules) lives in a separate, private plugin. Install both side-by-side; they don't overlap.

## Skills in this plugin

> _(none yet — initial scaffold; first skills land in 0.1.x)_

Planned skills:

| Skill | When it triggers |
|---|---|
| `writing-iec-61131-3-st` | When generating new Structured Text — picks the right idioms for the IEC standard, guards against language quirks (BYTE-vs-USINT, signed-vs-unsigned wraps, `MOD` semantics, `:=` vs `=`, `{attribute ...}` placement). |
| `designing-iec-61131-3-function-blocks` | When designing a new FB — VAR_INPUT vs VAR_IN_OUT vs VAR_OUTPUT vs VAR rules, `THIS^` semantics, `INTERFACE` vs abstract `FUNCTION_BLOCK`, `EXTENDS` rules, `METHOD` visibility, `PROPERTY` getter/setter shape. |
| `using-oscat` | When the user references OSCAT or asks for a stock industrial pattern (PID variants, motor control, signal generators, math helpers) — points at the right OSCAT FB and gives the call-site signature. |
| `iec-61131-3-pitfalls` | When code review surfaces classic IEC traps — TON not retriggering, R_TRIG/F_TRIG state across cycles, REAL `=` comparisons, ARRAY of struct alignment surprises, division-by-zero behaviour. |

These names are placeholders for now. The first skill written will be informed by what's actually painful in real-world use.

## Install

Two-line install (this repo is its own one-plugin Claude Code marketplace):

```
claude plugin marketplace add https://github.com/phobicdotno/phobic-iec-61131-3-skill.git
claude plugin install phobic-iec-61131-3-skill@phobic-iec-61131-3
```

Restart Claude Code. Skills auto-trigger by their `description:` frontmatter.

**Update later:**
```
claude plugin marketplace update phobic-iec-61131-3
claude plugin update phobic-iec-61131-3-skill@phobic-iec-61131-3
```

## Adding a new skill

1. Create `skills/<name>/SKILL.md` (verb-first folder name for procedural skills — `writing-X`, `designing-Y`, `using-Z`).
2. YAML frontmatter must have `name` and `description`. Description starts with **"Use when..."** and describes ONLY trigger conditions, never the workflow.
3. Add a row to the Skills table above.
4. Bump `version` in `.claude-plugin/plugin.json` AND `.claude-plugin/marketplace.json` (must match).
5. Commit + push.

See the upstream `superpowers:writing-skills` skill for the TDD-for-skills authoring loop (RED baseline → GREEN minimal skill → REFACTOR closing rationalisations with subagents).

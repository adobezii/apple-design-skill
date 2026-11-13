# AGENTS.md

Operating manual for agents using or maintaining this repository.

## What this repository is

`SKILL.md` is the `apple-web-craftsmanship` skill: a decision system for
high-craft web interfaces. It combines Apple-inspired typography, dark-mode
hierarchy, Liquid Glass boundaries, interruptible motion, and 1992 HIG
cognitive structure.

The `references/` directory is retained from the upstream fork so no inherited
material is lost. The active workflow is defined by `SKILL.md`; do not treat
inherited reference files as required reading unless the user explicitly asks
for them.

## Working with the skill

1. Read `SKILL.md` in full before applying its rules.
2. Classify the interface as A, B, or C before choosing any visual or motion
   treatment.
3. Keep A/B expressiveness out of C data surfaces.
4. Use concrete CSS values and framework code rather than describing an effect
   vaguely.
5. Run the verification hook in section 7 of `SKILL.md` before delivery.

## File map

| Path | Role |
| --- | --- |
| `SKILL.md` | The active skill and verification contract |
| `agents/openai.yaml` | Codex UI metadata and invocation policy |
| `README.md` | Human-facing overview and install instructions |
| `.cursorrules` | Cursor entry point |
| `GEMINI.md` | Gemini or Antigravity entry point |
| `references/` | Inherited upstream reference material |
| `scripts/` | Inherited upstream maintenance scripts |

## Checks

```bash
rg -n '^name: apple-web-craftsmanship$' SKILL.md
rg -n '^## 7\. 輸出前自檢閉環' SKILL.md
npx skills add . --list
```

Before committing, confirm that `SKILL.md`, `agents/openai.yaml`, and the entry
point files use the same skill name and invocation prompt.

# Apple Web Craftsmanship

An agent skill for building and reviewing polished web interfaces with Apple-level
typography, motion, material hierarchy, and cognitive structure.

This fork started from
[dickwu/apple-design-skill](https://github.com/dickwu/apple-design-skill). Its root
`SKILL.md` now contains the `apple-web-craftsmanship` version maintained by
[adobezii](https://github.com/adobezii).

## Install

```bash
npx skills add adobezii/apple-design-skill -a codex -g
```

Or clone it directly:

```bash
git clone https://github.com/adobezii/apple-design-skill.git \
  ~/.codex/skills/apple-web-craftsmanship
```

## What it covers

- A three-tier decision model separating brand pages, navigation and gesture
  surfaces, and dense monitoring or data interfaces.
- Optical typography, CJK spacing, line-breaking, and punctuation correction.
- Dark Mode elevation, semantic colors, concentric corners, and Liquid Glass
  boundary rules.
- Interruptible motion, velocity handoff, spring conversion, momentum
  projection, and rubber-banding.
- Copy-ready CSS, TypeScript physics, and React sheet examples.
- A verification checklist for generated and reviewed interfaces.

The skill intentionally distinguishes expressive interface chrome from dense
operational UI. Do not apply decorative typography, springs, or glass effects to
data surfaces where they reduce scanability.

## Repository layout

```text
apple-design-skill/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── AGENTS.md
├── GEMINI.md
├── .cursorrules
└── references/        # Retained from the upstream fork
```

## Source

The guidance is distilled from Apple's public Human Interface Guidelines and WWDC
material on fluid interfaces, Dark Mode, typography, and Liquid Glass. This
project is not affiliated with or endorsed by Apple.

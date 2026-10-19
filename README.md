![Agent Design Principles](assets/banner.svg)

# agent-design-principles

Design principles for products where an AI agent proposes, executes or monitors actions for a person. Written as reusable skills for AI coding assistants, and as plain Markdown anyone can read.

All examples use a fictional company and product (Acme Field Sales Assistant). No real customers, data or systems appear in this repo.

## Skills

| Skill | Question it answers |
|---|---|
| [agentic-ux](skills/agentic-ux/SKILL.md) | How does a screen make an agent visible, controllable and reversible? |
| [write-contract](skills/write-contract/SKILL.md) | Does this control have somewhere to land in the system of record? |
| [dense-operations-ui](skills/dense-operations-ui/SKILL.md) | How should a screen look when people use it all day to decide? |
| [autonomy-tiers-and-evals](skills/autonomy-tiers-and-evals/SKILL.md) | When has an agent earned more autonomy? |

## Also here

- [docs/principles.md](docs/principles.md): the ideas and how the skills connect, with diagrams
- [templates/](templates): a screen audit and a write contract table
- [examples/acme-sales-assistant](examples/acme-sales-assistant): both templates, filled in

## Use

Copy a folder from `skills/` into your assistant's skills directory, or read the files directly.

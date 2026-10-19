# Sources

Public work this method builds on, and what it takes from each. None of these authors reviewed or endorsed this repo.

| Source | What the method takes from it |
|---|---|
| [Microsoft HAX Toolkit](https://www.microsoft.com/en-us/haxtoolkit/), Guidelines for Human-AI Interaction | The frame behind `agentic-ux`: make clear what the system can do and how well it can do it, support efficient dismissal and correction, learn from user behavior, remember recent interactions |
| [Google PAIR, People + AI Guidebook](https://pair.withgoogle.com/guidebook/) | Explainability and trust, errors and graceful failure, feedback and control: calibrate confidence per item, explain in layers, design the failure states, make feedback visibly change behavior |
| [Shape of AI](https://www.shapeof.ai/) | A catalog of AI interface patterns, used to keep the vocabulary of `agentic-ux` recognizable to designers who already use it |
| [Teresa Torres, AI Evals: A Hands-On Guide for Product Teams](https://www.producttalk.org/ai-evals/) (Product Talk, 2026) | The product team owns the definition of correct; start from a few real errors; prefer the simplest measurement that works, code assertions before golden datasets before a model as judge; set a baseline, then re-check every eval after each change |
| [Rick Pollick, Evals Are the New Acceptance Criteria](https://rickpollick.com/blog/evals-are-the-new-acceptance-criteria) (2026) | Acceptance is a threshold on a distribution, not a checkbox; thresholds per feature and risk tier, calibrated before they gate; evals measure the average while guardrails protect the tail; production failures feed the golden set |
| [Critical Path, If you can't write the eval, you can't ship the AI feature](https://buttondown.com/critical-path/archive/if-you-cant-write-the-eval-you-cant-ship-the-ai/) | The title is the rule: an AI behavior you cannot write the eval for is not ready to ship, which is why every tier change here starts from the cases |
| [Chen Ye Zhu, Golden Eval Sets: Behavioral Product Specs for Production GenAI](https://chenyezhu.com/writing/golden-eval-set-prd-genai-system/) | The golden set as a behavioral spec: for an agent, each case states the expected action, the forbidden actions and the risk level, scored with deterministic tool checks before any rubric |
| [Marty Cagan, The AI Productivity Paradox](https://www.svpg.com/the-ai-productivity-paradox/) (SVPG, 2026) | Faster building does not improve outcomes on its own; separate building to learn from building to earn, which is why autonomy here is a decision backed by evidence rather than by a demo |

## Where the method departs from them

- The three regimes shown on screen (acts alone, proposes and waits, alerts only) and the five tiers used to earn them are this method's own simplification, chosen so a card can carry one signal and a sponsor can read one ladder.
- The write contract is a systems rule, not a UX pattern. None of the sources above make "a control must have a destination in the system of record" a scoping rule; this method does, because it is the cut that most often decides whether an agent product delivers.
- Every number in the skills (set sizes, thresholds, windows) is illustrative. The sources give ranges; the method asks you to set your own per action type and risk.

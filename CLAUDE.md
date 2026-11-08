# QA workspace — {Product name}

<!-- Edit this file: name the product, set the language, add anything the agent should always know. -->

## Product
{One sentence: what the product is and who uses it.}

## Language
Write QA artifacts (notes, test cases, reports) in {English}. Talk to me in {English}.

## Rules
- Product repositories under `repos/` are read-only: read code, history and docs there; never edit, commit or push.
- QA artifacts go to `qa-work/` (layout in `feature-flow.toml`). Commit them in this workspace repository.
- Never put secrets into chat or into committed files: name the file and the variable that holds them.
- Do not run the product, builds, containers or package managers unless I ask.

## How to start
1. First time: `/qa-feature-flow:qa-feature-flow setup` — scans this folder and asks what it cannot find.
2. Then, per feature: `/qa-feature-flow:qa-feature-flow {feature} {platform}`
   (`{feature}` — a ticket key or a feature name; `{platform}` — `web`, `mobile` or `both`).
   One phase per session: start a fresh session for the next phase.

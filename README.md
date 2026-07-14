# knowledge-base-constructor-en

**The idea — an LLM wiki.** A project's sources are compiled once into a permanent, interlinked wiki; from then on Claude answers by reading it instead of re-deriving the same things from raw documents on every question.

A single source from which such LLM wiki projects are assembled for different needs (saas / research / business) and any combination of mechanics (claim graph, lifecycles, code). One source instead of template copies — versioned per part, no drift.

- **Where it lives:** public mirror — [github.com/DanilZherebtsov/knowledge-base-constructor-en](https://github.com/DanilZherebtsov/knowledge-base-constructor-en). (Русская редакция: [knowledge-base-constructor](https://github.com/DanilZherebtsov/knowledge-base-constructor).)
- **Get started:** clone the [public mirror](https://github.com/DanilZherebtsov/knowledge-base-constructor-en) into an empty repository and just start a conversation in Claude Code — the constructor assembles the project through an interview (you end up with a filled-in `CLAUDE.md` and the wiki structure; the constructor scaffolding removes itself). See [START-HERE.md](START-HERE.md).
- **Contents:** `base/` (skeleton + universal methodology) · `mechanics/` (detachable mechanics) · `presets/` (class manifests) · [`versions.json`](versions.json) (part versions).
- **Versions and updates:** each part is versioned independently. An assembled project carries a **fingerprint** of the versions used (line 3 of its `CLAUDE.md`) and checks them against `versions.json` during lint — it hears only about the parts it actually uses. Mechanics — [ASSEMBLY.md](ASSEMBLY.md) and [base/methodology/lint.md](base/methodology/lint.md).

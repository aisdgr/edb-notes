# Governance Must Shift Left — Design Principles, Boundaries, and the CODE Rule Catalog

Across the first three articles, we built BRA from the ground up: P1 established why governance must shift left, P2 separated intent from enforcement through NNL and RNL, and P3 turned Rule into a structured governance asset with the Three Iron Laws. Now we return to a fundamental question: how do design principles produce actual constraint force? How do abstract concepts become verifiable engineering assets?

This is the final article in the series. If the first three articles constructed the skeleton of BRA, this article demonstrates that the skeleton stands.

---

## If It Is Injected, It Is Execution

The most common failure mode in AI behavior governance is not that "the rules are not strict enough." It is that governance information gets misread as execution directives.

### The Problem

Consider a seemingly reasonable Rule metadata design:

```yaml
risk: high
severity: critical
approval_required: true
```

The developer's intent is clear: mark this Rule as important so governance workflows know to intervene. But when this information enters the AI's execution context, an LLM might interpret it differently:

- `risk: high` means this is important, so prioritize it.
- `severity: critical` means process it faster.
- `approval_required: true` means find a way to bypass the approval.

This is not an LLM bug. It is a structural problem: **there is no semantic boundary between governance attributes and execution directives.**

### Attribute vs Imperative Strict Separation

One of BRA's core design principles uses expression form, not content, to define the governance boundary:

> **If it is injected, it is execution. If it is an attribute, it is governance.**

The practical implication is unambiguous. All information within a Rule, based on its expression form, serves exactly one of two roles:

| Type | Expression Form | Audience | Role |
|------|----------------|----------|------|
| **Attribute** | Descriptive, structured metadata | Governance toolchain / Human review | Governable fact |
| **Imperative** | RNL-expressed behavioral requirement | AI agent | Executable directive |

The correct approach structures governance attributes explicitly, avoiding any format that could be misread as a behavioral instruction:

```yaml
attributes:
  risk_potential:
    impact_score: 4
    rationale: affects_release_safety
  behavior_characteristics:
    autonomy_level: low
    requires_human_attention: true
```

`risk_potential` uses `impact_score` plus `rationale` as structured description, rather than `risk: high` as a tag. The former is an attribute for governance machinery to read; the latter is an injection that AI might reinterpret as execution priority.

### Context

This separation resolves more than a formatting issue. It establishes a deeper engineering discipline: **governance does not depend on what is described, but on how it is expressed.** The same risk information, expressed as an attribute, is governance. Expressed as an injection, it is execution contamination.

The risk handling in BRA follows a three-layer model that enforces this separation: `risk_potential` (prior assessment at design time) lives in the Rule as a descriptive attribute; `risk_signal` (runtime observation) belongs to BCM; `realized_risk` (audit judgment) belongs to governance review. Each layer has its own expression form, its own audience, and its own role. Mixing them collapses the boundary.

### Implication

This principle matters because it prevents a subtle but pervasive failure: governance leakage. When attributes leak into execution context, AI systems do not become more governed — they become more confused. The separation ensures that Rule remains a stable governance artifact whose meaning does not shift depending on which system consumes it.

---

## Struct Semantics — Reducing Degrees of Freedom, Not Increasing Command Force

### A Counterintuitive Finding

Intuition suggests that strengthening AI behavior constraints means writing stronger instructions. But empirical observation reveals a different reality:

> **Rules fail not because models refuse them, but because models reinterpret them.**

BRA's Struct Semantics mechanism operates on a different assumption: effectiveness comes from **reducing degrees of freedom**, not from increasing command force.

### The Semantic Room Model

Rule structure is not a data format — it is a set of semantic boundaries. Each Struct Semantic constrains a different semantic dimension:

| Struct Semantic | Semantic Dimension Constrained | Effect on LLM |
|----------------|-------------------------------|---------------|
| WHY (intent) | What this Rule is for | Locks interpretation direction |
| WHO (roles) | Who should comply | Narrows applicable subjects |
| WHAT (applies_to) | Which outputs are relevant | Limits scope of effect |
| WHEN (context) | Under what circumstances relevant | Narrows trigger conditions |
| WHAT HAPPENS (effect) | Semantic consequence of violation | Locks behavioral expectation |

> **LLMs are not commanded; they are confined in a smaller semantic room.**

This insight is particularly effective for GPT-4.1+ and comparable models. These models treat structure as semantic boundary lines, not merely data formatting: a YAML key becomes a semantic anchor, hierarchy implies priority and scope, and a capsule defines an interpretation domain.

### Context

The mechanism has limits. Struct Semantics produces no behavioral effect when the model is overridden by a system prompt, the constraint itself is ambiguous, or the structured semantics are never projected into the execution prompt. Schema plus correct projection is necessary.

### Comparison

| Method | Mechanism | Effect |
|--------|-----------|--------|
| Stronger prompts (more commands) | Relies on LLM willingness to comply | Unstable, easily overridden |
| Reduced freedom (Struct Semantics) | Shrinks the space of valid interpretation | Structurally stable |

### Implication

This finding inverts the conventional approach to AI constraint engineering. Instead of asking "how do I command the AI more effectively?", the question becomes "how do I reduce the space within which the AI can reasonably diverge?" The shift from commanding to confining is not merely rhetorical — it reflects a fundamentally different engineering strategy.

---

## Boundary Architecture — Two-Layer Design

### Why Boundary Matters

Rule defines behavioral constraints, but a Rule does not know the context in which it will be used. A Rule stating "new code MUST include JWT authentication" has completely different applicability in API development versus data analysis scripts.

Boundary's role is to **define Decision Scope** — declaratively, non-executively, and non-normatively. It does not tell AI what to do. It tells the governance system "within which scope this Rule is considered."

### The Two Layers

BRA adopts a dual-layer Boundary design, separating static semantics from dynamic execution:

**Layer 1: Rule-level Boundary (Static Declaration)**

The `boundary` field within a Rule's structure declares the Rule's semantic applicability:

```yaml
boundary:
  change_type:
    - new_code
  feature_scope:
    - authentication
  security_mechanism:
    - jwt
```

This is the Rule author's static declaration at design time. Boundary properties are entirely customizable by the enterprise or domain. They declare semantic scope — not runtime behavior.

**Layer 2: Execution Boundary (Dynamic Binding)**

An external controller defines the Runtime Scope, dynamically binding Rule-level Boundaries to concrete execution environments: file paths, tool contexts, project configurations.

- Rule boundary defines **semantic applicability**.
- Execution boundary defines **runtime binding**.

### Context

The two-layer separation maps directly to BRA's architectural discipline. Rule-level Boundary lives inside BRA — it is part of the structured governance artifact. Execution Boundary lives outside BRA, handled by the integration layer. This separation ensures that Rule remains independently composable while scope binding adapts to the execution environment.

### Implication

A single Rule can be activated or deactivated across different Execution Boundaries without modifying the Rule itself. The same Rule, authored once, serves multiple contexts. This is how Rule becomes a cross-stage governance asset — not by being rewritten for each stage, but by being bound differently at each stage.

---

## BCM — BRA's Governance Complement

### Positioning

Everything discussed so far concerns static structure. But governance is a dynamic process. BRA needs a complementary framework to handle runtime governance logic.

**BCM (Behavior Control Model)** fills this role:

| Dimension | BRA | BCM |
|-----------|-----|-----|
| Nature | Engineering architecture | Governance framework |
| Output | Static structure (Rule, Ruleset) | Dynamic cycle (PDCA) |
| Focus | Define behavior constraints | Enforce, observe, adjust |

### The Complete Control Chain

BRA and BCM together form the full governance control chain:

```
VSS → Boundary → BRA → BCM
```

- **VSS (Visible Scope):** Defines what AI can see.
- **Boundary:** Defines Behavior Scope, specifies applicable Rulesets.
- **BRA:** Provides Rule, Ruleset, and static governance assets.
- **BCM:** Handles Runtime Enforcement, Risk Signal Detection, Evidence Generation, and the PDCA governance cycle.

### Comparison

| Concern | BRA Handles | BCM Handles |
|---------|-------------|-------------|
| Constraint definition | Yes | No |
| Constraint interpretation guidance | Yes | No |
| Runtime violation detection | No | Yes |
| Risk signal identification | No | Yes |
| Audit evidence generation | Enables | Produces |
| Governance adjustment loop | No | Yes |

### Implication

The separation between BRA and BCM is not an architectural deficiency — it is a design discipline. **BRA is solely responsible for "defining behavior constraints." It does not execute, judge, govern, or validate.** This ensures Rule remains a stable governance artifact whose semantics do not shift with execution environment changes. Neither is sufficient alone. Together, they form a complete governance system.

---

## CODE Rule Catalog — From Concept to Asset

126 rules. This is the critical step where BRA moves from abstract architecture to concrete asset.

### Five Non-Negotiable Design Principles

The CODE Rule Catalog follows five non-negotiable design principles, each directly reflecting the design logic established across this series:

1. **Code-Level Only** — Rules apply exclusively to code artifacts, avoiding scope creep into other artifact types.
2. **Scenario-Free** — Rules do not encode add/change/fix/refactor intents, preventing semantic coupling with task context.
3. **Boundary-First and Fail-Stop** — When boundary is missing, conflicting, or invalid, the system MUST stop immediately.
4. **Composable and Reusable** — Every rule is atomic and reusable across Rulesets.
5. **Execution Rules vs. Governance Guardrails** — MUST (engineering obligations) and MUST NOT (hard prohibitions) are explicitly separated.

Principle 5 directly validates the Policy vs Constraint distinction from P3. Execution Rules (MUST) define engineering obligations where violations are reparable. Governance Guardrails (MUST NOT) define hard prohibitions where violations constitute evidence-based governance triggers that MUST stop execution, log, and report.

### Seven Categories Across Four Domains

The 126 rules span four Domains (CODE, DOCS, LANG, GOV) and seven Decision Behavior Categories:

| Category | Name | Risk Domain Focus |
|----------|------|-------------------|
| TR | Traceability | Tracking identifiers and non-orphaned code |
| BD | Boundary and Stop | Execution boundaries and hard stop conditions |
| AR | Artifact Isolation | Cross-artifact contamination prevention |
| ST | Structural Change | Structural and behavioral integrity |
| TI | Test Integrity | Test artifact misuse prevention |
| CN | Constraint Neutrality | Over-inference and scope expansion |
| LG | Logging and Report | Execution transparency and evidence completeness |

The BD category consists entirely of Governance Guardrails (MUST NOT). This is by design — boundary protection is governance's first line of defense. Every risk domain is required to contain at least one MUST NOT, ensuring that each category has an observable behavioral boundary.

### Context

The CODE Rule Catalog validates several BRA concepts simultaneously. Decision Behavior Category proves that classifying by AI decision behavior type, rather than by application domain, produces clear and non-overlapping categories. The naming convention (`CODE-<CATEGORY>-<NN>`) demonstrates Rule IDs that remain stable regardless of Ruleset composition. The catalog's evaluation score of 8.4/10 reflects its role as the strongest evidence that abstract concepts can be materialized into concrete, shareable governance assets.

### Comparison with Abstract Governance

| Dimension | Abstract governance statements | CODE Rule Catalog |
|-----------|-------------------------------|-------------------|
| Interpretability | High ambiguity, model-dependent | Structured RNL, minimal ambiguity |
| Composability | No systematic composition | Atomic rules, dynamic Ruleset assembly |
| Observability | Cannot generate evidence | MUST NOT violations produce audit evidence |
| Reusability | Typically one-time use | Cross-project, cross-industry, cross-stage |
| Lifecycle | No versioning | Versioned, from draft to active to deprecated |

### Implication

The catalog demonstrates that Rule Library is not a theoretical concept but an operational asset. Rules use stable IDs (such as `CODE-BD-03`) that persist regardless of Ruleset composition. The same Rule can serve in Development, CI/CD, and Runtime with different roles at each stage.

126 rules is not an endpoint but an extensible taxonomy. Future domains use independent namespaces, integrating into the same Rule Library.

---

## Convergence — Governance Shift-Left as Action Cognition

Return to the core proposition from P1: governance must shift left.

### From Concept to Action

Four articles have established a complete chain of reasoning:

1. **P1** defined the problem: LLM creativity conflicts fundamentally with engineering predictability. Governance failure is not caused by insufficient control, but by governance arriving too late.
2. **P2** provided the language foundation: NNL optimizes intent, RNL enforces behavior. Semantic separation is the first step.
3. **P3** built the asset model: Rule is the atomic governance unit, Rule Library is a shareable repository, Ruleset is the composition layer governed by the Three Iron Laws.
4. **P4** (this article) proved the practical force: Attribute vs Imperative separation prevents governance leakage. Struct Semantics produces structural constraint by reducing degrees of freedom. Boundary Architecture enables scope control without modifying Rule content. CODE Rule Catalog materializes 126 rules from abstract concepts into concrete governance assets.

### Three Action Cognitions

**First, Rule is a cross-stage governance asset.** It is not a prompt fragment or a configuration entry. Rule is a structured governance unit reusable from development through to runtime. The concrete action of governance shift-left is to structure behavioral constraints as Rules at the development stage — before AI executes.

**Second, structure is more reliable than instruction.** Attribute vs Imperative separation, Struct Semantics, and the two-layer Boundary design share a common logic: do not attempt to control AI with stronger commands, but shrink the space within which AI can reasonably diverge. The engineering choice is between commanding an uncertain system and confining it within defined boundaries.

**Third, composition authority belongs to Application, not Category.** Behavior Category describes the type of AI decision behavior. Rule defines the specific behavioral constraint. But the final decision — which Rules are used and how they are composed — belongs to Application Ruleset. Category is the entry point for selection, not the endpoint for relationships.

### Closing

> Risk without prescribed behavior is not governance; it is merely annotation.
>
> Categories describe behavior; Rules enforce constraints; Applications decide composition.

LLMs are highly creative, uncertain systems. Engineering systems require predictable behavior. BRA does not attempt to eliminate LLM creativity. Through structured behavioral constraints, it establishes an engineering-controllable boundary between creativity and predictability.

Governance shift-left is not a slogan. It is an engineering choice: write the constraints clearly before AI executes.

---

*This is the final article in the Behavior Rule Architecture series. Previous articles: [P1 — When AI Enters Execution, Engineering Starts Losing Responsibility](/p1-link), [P2 — Two Languages, One Problem](/p2-link), [P3 — Rule as a Governance Asset](/p3-link).*

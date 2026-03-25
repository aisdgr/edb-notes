# Rule as a Governance Asset — Structure, Classification, and the Three Iron Laws

This is the third article in the Behavior Rule Architecture (BRA) series. In previous posts, we established why governance must shift left (P1) and how NNL and RNL achieve semantic separation (P2). Now we enter the most technically dense part of the series. We examine what a Rule is, how it is structured, why Policy and Constraint are fundamentally different, and how Rules become reusable governance assets.

---

## The Anatomy of a Rule: Four Components, One Normative Core

A Rule in BRA is not a prompt snippet or a configuration flag. It is a structured governance artifact — a single, atomic unit designed to express mandatory behavioral boundaries to AI in a stable, inspectable, and reusable form.

Formally:

**Rule = (RNL, Governance Metadata, Boundary_Declaration)**

Every Rule consists of four components. Only one carries enforceable meaning.

### 1. Policy or Constraint — The Normative Core

The only normative component. Written in RNL using MUST or MUST NOT keywords, it defines a single behavioral directive. All behavioral meaning must be derivable from this field alone.

### 2. Execution View — The Interpretation Lens

An optional field defining how the AI should interpret the normative core. Not a role assignment or authority declaration — purely an interpretation filter to reduce ambiguity. For example, `execution_view: qa` signals a quality-assurance reading lens.

### 3. Boundary — The Decision Scope

Declares where the Rule applies: which artifacts, which sources, which domains. It is declarative, non-executable, and non-normative. It defines where the Rule is relevant, not when or how it triggers. Concrete bindings are supplied externally at execution time.

### 4. Governance — The Descriptive Context

Provides WHO, WHY, WHEN, and WHAT HAPPENS context: intent, responsibilities, applicability context, and governance impact. Every field is informational only — it must never participate in execution behavior.

### Normative Exclusivity and the Minimal Rule

The principle of **normative exclusivity** governs this structure: Policy or Constraint defines behavior; everything else explains context. No metadata field may impose mandatory behavior or introduce alternative interpretations.

The smallest valid Rule needs only an identifier and a normative statement:

```yaml
rule:
  id: RULE-001
  constraint: >
    AI MUST stop generation when required input is missing.
```

This is already a valid AI constraint, copyable into a prompt, understandable by humans, and interpretable by systems. Everything beyond this exists to reduce ambiguity at scale.

---

## Two Design Principles: Norm Over Control, Declarative Not Procedural

Two principles shape every Rule, distinguishing BRA from procedural control systems.

**Norm Over Control**: Rules define boundaries, not procedures. A Rule tells the AI what must or must not hold — not how to achieve it. When you write a Rule, you are not writing an instruction manual. You are drawing a line.

**Declarative, Not Procedural**: A Rule describes what must hold, not how to act. No IF/THEN branching, no execution steps. Procedural content invites the AI to reinterpret the Rule as a suggestion rather than a boundary. Declarative statements leave less room for semantic drift.

**Implication**: If you find yourself writing "if X happens, then do Y" inside a Rule, you have left the declarative domain. That logic belongs to an execution layer.

---

## Policy (MUST) vs Constraint (MUST NOT): Expectation vs Restriction

In early BRA design, "Constraint" was used as a blanket label for all normative statements. Implementation revealed that MUST and MUST NOT behave fundamentally differently in practice. BRA now treats Rule as the top-level concept with two subtypes:

- **Policy** (MUST) — an expectation of behavior
- **Constraint** (MUST NOT) — a restriction on behavior

### The Observable Differences

| Dimension | Policy (MUST) | Constraint (MUST NOT) |
|---|---|---|
| **Nature** | Expectation | Restriction |
| **Trigger Observability** | Not directly observable | Observable — violation is detectable |
| **Evidence** | No direct evidence produced | Violation produces concrete evidence |
| **Enforcement** | Medium strength | High strength |
| **Audit Strategy** | Positive verification | Negative verification |

### Context: Why This Matters

Consider a **Policy**: "AI MUST include trace metadata in every generated artifact." When AI complies, the output is correct — but compliance itself is not directly observable. No automatic signal confirms the Rule was followed. Evidence is indirect.

Now consider a **Constraint**: "AI MUST NOT modify files outside the declared artifact boundary." When AI violates this, the violation is immediately detectable. An unauthorized modification is a concrete, observable event that generates evidence — a violation log, a behavioral deviation record.

This aligns with safety-critical systems theory. Policy resembles a liveness property (something that should happen). Constraint resembles a safety property (something that must never happen). Safety violations are always observable; liveness violations are not.

### Implication

This classification determines the verification strategy: Policy requires output review and compliance checking (post-hoc). Constraint requires runtime detection and violation evidence (real-time). The enforcement mechanism and evidence model differ fundamentally.

---

## Decision Behavior Category: Behavior Before Domain

Conventional approaches design Rules first, then map them to domains. BRA inverts this. The starting point is not the domain — it is the behavior.

A **Decision Behavior Category** classifies *how AI decision behavior should be normatively constrained*, not *what domain the AI operates in*. You define the behavioral norm type first, then derive Rules from it.

The practical consequence: the same category applies across domains. Traceability Rules are relevant whether you generate code, write documentation, or produce test cases. The behavior type is the same; the domain is incidental.

This contrasts with **Domain Category** approaches, where a single Rule often intersects multiple domains simultaneously. Domain boundaries blur. Behavioral boundaries do not.

### Examples from the CODE Rule Catalog

The CODE Rule Catalog — 126 Rules across four domains — demonstrates seven Decision Behavior Categories:

- **AR (Artifact Isolation)**: Maintaining independence of produced artifacts
- **BD (Boundary & Stop)**: Operating within declared boundaries and halting on violation
- **CN (Constraint Neutrality)**: Ensuring Rules themselves are unambiguous
- **TR (Traceability)**: Preserving execution trails and audit chains
- **LG (Logging & Report)**: Recording critical operations
- **ST (Structural Change)**: Controlled modification of existing structures
- **TI (Test Integrity)**: Maintaining test independence and validity

Every Category must contain at least one Constraint (MUST NOT). A category with only Policies can describe what should happen but cannot detect what went wrong. A category with a Constraint can always produce violation evidence.

Each Rule belongs to exactly one Behavior Category (1:N relationship). Category Rulesets are mutually exclusive. When a new domain emerges, you do not start from scratch — you ask which Decision Behavior Categories apply, then derive Rules accordingly.

---

## Rule Library and Rule Category: Governance as Reusable Asset

When Rules are structured, classified, and versioned, they become something most AI governance lacks: **assets**.

Currently, behavioral constraints are scattered across prompts, custom instructions, and configuration files. They cannot be inventoried, versioned, or shared. A **Rule Library** centralizes all Rules as first-class governance artifacts — reusable across development, CI/CD, and runtime stages, shareable across projects and enterprises.

The evolution path is natural: **Rule Pool** (single project) -> **Repository** (organization-wide) -> **Marketplace** (cross-organizational).

A critical design decision: **Categories describe behavior but do not compose Rules**. A Category tells you "these Rules are about traceability." It does not tell you "use these Rules together." The Library is the union of all Category Rulesets. Composition decisions belong elsewhere — to the Application Ruleset.

---

## Ruleset: The Governance Envelope

A **Ruleset** is a governed grouping of Rules — a governance envelope that gives multiple Rules a shared identity without changing what any Rule means. It is not normative. It does not define, modify, or override constraints.

The smallest valid Ruleset:

```yaml
ruleset:
  id: RULESET-001
  includes:
    - rule_id: RULE-001
```

Rulesets reference Rules by identifier. They do not inline, copy, or redefine them.

### Two Types of Ruleset

**Category Ruleset**: All Rules belonging to the same Decision Behavior Category. Rule to Category Ruleset is 1:N.

**Application Ruleset**: Composed according to any organizational principle — workflow-based, domain-based, risk-based, compliance-based, or custom. Principle types are unlimited. Rule to Application Ruleset is N:N.

Structural constraints hold across the architecture: every Rule belongs to one Category, can be referenced by many Application Rulesets, and all Category Rulesets combined form the complete Library. A Ruleset is greater than one Rule, but less than a governance system.

---

## The Three Iron Laws of Ruleset Design

Application Ruleset flexibility introduces a risk: without constraints, the composition layer becomes chaotic. Categories might suggest compositions. Rulesets might define new behavior. To prevent this, BRA defines three inviolable principles.

### Iron Law 1: A Ruleset MUST NOT Define Policy or Constraint

A Ruleset references Rules. It does not create them. All normative behavior remains inside individual Rules. A Ruleset containing new behavioral directives is no longer a Ruleset — it is an improperly decomposed Rule.

### Iron Law 2: A Ruleset Is a Lens, Not a Controller

A Ruleset answers "which Rules should be considered?" — not "which Rules should be executed?" It is a viewpoint, not a runtime controller. The same Ruleset may be relevant to ten execution contexts but active in only three. The Ruleset declares relevance; the execution layer decides activation.

### Iron Law 3: A Ruleset MUST Be Reusable Across Systems

If a Ruleset works only for one tool or team, it has failed as a governance artifact. The same Ruleset should be interpretable by a CI/CD system, a development tool, and an audit platform without modification.

### The Responsibility Boundaries

| Layer | Responsible For | Must NOT Do |
|---|---|---|
| **Decision Behavior Category** | Classifying Rules by behavioral norm | Define or suggest Rule composition |
| **Rule** | Defining a single Policy or Constraint | Know about its Rulesets |
| **Application Ruleset** | Composing Rules based on any principle | Define new Policies or Constraints |

---

## Correct Layering and Composition

The Iron Laws produce a clear layering principle:

**Language Layer -> Behavior Category -> Rule Layer -> Asset Layer -> Application Ruleset**

Three composition principles enforce this structure:

**Composition Scope**: Rule composition occurs only within Application Rulesets. Behavior Categories do not define or enforce composition.

**Category Isolation**: Categories are responsible for classification only. The moment a Category recommends which Rules to use together, it has crossed into Application Ruleset territory.

**Atomic Rule Integrity**: Each Rule remains independently selectable and composable. The reference direction is one-way: Ruleset -> Rule. Inter-Rule relationships — mutual exclusion, dependency, grouping — belong to the Application Ruleset level.

### The Core Conclusion

> **Categories describe behavior; Rules enforce constraints; Applications decide composition.**

Categories provide the taxonomy. Rules provide the enforcement. Applications provide the contextual decision of which Rules to activate. Each layer knows its responsibility and stays within it.

---

In this article, we traced Rule from its minimal atomic form through Policy/Constraint classification, Decision Behavior Categories, the Rule Library, and the Three Iron Laws that govern Ruleset composition. The structure is complete. But how do these Rules connect to execution? How do Attributes stay distinct from Imperatives in practice? How do 126 real-world Rules demonstrate these concepts under pressure? In the next article, we move from architecture into engineering practice — the design principles, boundary architecture, and the CODE Rule Catalog evidence that shows governance shifting left.

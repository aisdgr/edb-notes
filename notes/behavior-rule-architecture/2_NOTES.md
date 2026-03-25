# BRA Early Design Material Distillation

## CORE TOPICS
- Architecture Scope and Philosophy
- NNL (Normative Natural Language)
- RNL (Rule Normative Language)
- Rule Structure and Types (Policy vs Constraint)
- Ruleset Design
- Boundary Architecture
- Governance Metadata
- Consensus Decisions from AI Assistants

## STRUCTURED NOTES

### Architecture Scope and Philosophy

#### Concept
BRA (Behavior Rule Architecture) defines behavior constraints at development stage, strictly separated from execution, judgment, governance, or validation.

#### Detail
**Three-Layer Architecture:**
- BRA: Rule definition (development stage)
- BCM (Behavior Control Model): Rule execution (runtime stage)
- Boundary Architecture: Scope binding (application stage)
- External Tools: Validation and management

**Core Philosophy:**
- BRA: Define rules, not execute/judge/govern/validate
- Responsibility separation: Language Layer → Rule Architecture Layer → Governance Asset Layer

**Key Principles:**
- Behavior constraint priority
- Governance and execution separation
- Rule independence
- Semantic clarity
- Practical feasibility
- Future extensibility

#### Implicit Assumptions
- Structured representation reduces AI interpretation ambiguity
- Declarative approach superior to procedural for governance

---

### NNL (Normative Natural Language)

#### Concept
Soft specification layer reducing AI guesswork without fixed keywords.

#### Detail
**Core Principle:**
> Important things should not be left for AI to guess.

**Three Intent Types:**
1. Goal Clarification: Clarify actual objectives
2. Important Condition Signaling: Emphasize non-negligible conditions
3. Expected Result or Behavior: Define success/failure outcomes

**Design Philosophy:**
- Not controlling AI behavior, but reducing guesswork
- Relies on normative intent, not fixed keywords
- Violation not a syntax error, but makes responsibility attribution visible

#### Example
"Help me implement login" → "I want a [Login Page]"

---

### RNL (Rule Normative Language)

#### Concept
Controlled specification language making AI harder to deviate from boundaries.

#### Detail
**Core Principle:**
> Not making AI freer, but making AI harder to deviate from boundaries.

**Design Principles:**
1. Keyword Restriction: MUST / MUST NOT only (SHOULD returns to NNL scope)
2. Forbidden Vague Words: may / might / try / attempt / possibly / if possible
3. Sentence Structure: <Subject> <Normative Keyword> <Action> <Target> [Condition]

**Subject Selection Principle (Output-Oriented):**
- Should represent: Concrete artifacts, execution states, observable results
- Should not represent: Conversational personas, implied speakers/listeners

**Design Preference:**
> Prefer describing what must exist or change, not who must perform the action.

#### Example
```
Report MUST include a count of all warning-level findings.
Execution MUST stop when required input is missing.
Output MUST NOT contain data outside FILE_SCOPE.
```

#### Why RNL Effective
- Reduces AI interpretation ambiguity
- Improves cross-tool portability
- Supports structured validation

---

### Rule Structure and Types

#### Concept
Rule is smallest governance-bearing unit with single normative statement (Policy or Constraint).

#### Detail
**Four Core Components:**
1. Policy/Constraint (Normative Core): Only mandatory part
2. Execution View: AI's interpretation perspective
3. Boundary: Rule's decision scope
4. Governance: Descriptive governance context

**Rule Types (Key Distinction):**

| Type | Keyword | Nature | Trigger Observability | Evidence Generation | Enforceability |
|------|---------|--------|----------------------|---------------------|----------------|
| Policy | MUST | Expected behavior | Not observable | Requires evidence collection | Medium |
| Constraint | MUST NOT | Restrictive behavior | Observable | Can leave evidence | High |

**Design Principles:**
1. Single Rule, Single Normative: Each rule contains only one normative statement
2. Norm Over Control: Define boundaries, not procedures
3. Declarative, Not Procedural: Describe states to maintain, not how to act
4. Governance-Aware but Execution-Neutral: Metadata supports auditability but must not affect execution
5. Representation-Agnostic: YAML schema is reference representation, not standard itself

**Normative Exclusivity Principle:**
> Policy or Constraint defines behavior; everything else explains context.

Other fields must not:
- Impose mandatory behavior
- Override or weaken normative statement
- Introduce alternative interpretations

**Minimal Rule Concept:**
```yaml
rule:
  id: RULE-001
  policy: >
    AI MUST include tests when generating code.

  # Or
  constraint: >
    AI MUST NOT continue generation when required input is missing.
```

**Practical Significance:**
- Constraint (MUST NOT): Runtime enforcement possible
- Policy (MUST): Requires evidence collection and post-hoc verification

#### Terminology Evolution
Early design used "Constraint" as top-level concept; current design explicitly distinguishes:
- Rule: Top-level concept
- Policy: MUST statements (expected behavior)
- Constraint: MUST NOT statements (restrictive behavior)

---

### Ruleset Design

#### Concept
Controlled grouping of rules providing shared identity without changing rule semantics.

#### Detail
**Mental Model:**
> Ruleset = Controlled grouping of rules

**Design Principles:**
1. Rule Atomicity: Ruleset must not change rule intent
2. Aggregation Without Interpretation: Ruleset only groups, does not interpret
3. Governance Priority Structure: Ruleset exists to support governance, audit, lifecycle management
4. Execution Neutrality: Ruleset carries no execution semantics
5. Reference Not Duplication: Rules referenced by ID, not embedded or rewritten

**Mental Model (Governance Envelope):**
> Governance Envelope providing shared identity for multiple rules without changing any rule's substantive meaning.

**Role Definition:**
- Is: Named collection of rule references, constraint bundle within governance scope, version control unit
- Is Not: Defining new constraints, modifying rule semantics, executing/evaluating rules, resolving rule conflicts

**Category vs Application Ruleset Three-Layer Iron Law:**
1. Category MUST NOT define rule composition
2. Rule composition MUST occur only in application ruleset
3. Rules MUST remain atomic and independent

**Key Conclusion:**
> Categories describe behavior; Rules enforce policies and constraints; Applications decide composition.

**Ruleset Awareness:**
- Unidirectional reference (Ruleset → Rule)
- Rule unaware of Ruleset
- Maintains rule independence and reusability

---

### Boundary Architecture

#### Concept
Declares decision scope where constraint applies, defining WHERE rather than WHEN or HOW.

#### Detail
**Boundary Characteristics:**
- Declarative
- Non-executive
- Non-normative

**Two-Layer Model:**

**1. Rule-level Boundary (Static):**
- Located in Rule's `boundary` field
- Defines semantic applicability
- Purpose: Rule self-containment, library classification/query
- Example:
  ```yaml
  boundary:
    artifact_type: source_code
    source_ref: STS-JWT-001
  ```

**2. Execution Boundary (Dynamic):**
- Located in Boundary Architecture's external controller
- Defines runtime scope
- Purpose: Execution flexibility, supporting different scenarios
- Functions: Specify applicable Rulesets, runtime context binding, behavior scope constraints

**Key Definition:**
> Rule boundary defines semantic applicability; Execution boundary defines runtime binding.

**Design Rationale:**
- Balance rule self-containment with execution flexibility
- Rules reusable across different projects/scenarios
- Static declaration supports governance; dynamic binding supports execution

**Boundary properties entirely enterprise or domain defined.**

#### Example
```yaml
boundary:
  source:
    - sts: STS-JWT-001
    - sts: STS-JWT-003
  artifact:
    - source_code
    - test_result
```

---

### Governance Metadata

#### Concept
Descriptive metadata supporting future governance utilization without defining workflows, evidence logic, or enforcement mechanisms.

#### Detail
**Field Structure:**
- Version and lifecycle status
- Ownership (Owner)
- Intent and Rationale
- Responsibilities
- Applicability Context
- Governance Impact and Evidence Expectations

**WHO/WHEN/WHAT Framework:**

**Intent (WHY):**
- Describes governance rationale for Rule's existence
- May include `enterprise_domain` and `industry`
- Organization-defined, non-normative

**Responsibilities (WHO):**
- Declares stakeholder accountability, not execution roles
- Example:
  ```yaml
  responsibilities:
    ai:
      - generator
    human:
      - reviewer
      - gatekeeper
  ```
- Does not affect execution behavior, only for governance transparency

**Applicability Context (WHEN):**
- Describes governance-relevant applicability situations
- Important: Should not be interpreted as trigger conditions or execution logic
- Example:
  ```yaml
  applicability_context:
    scenario:
      test_failure:
        risk: quality
        severity: high
  ```

**Governance Impact (WHAT HAPPENS):**
- Describes expected governance-level outcomes
- Example:
  ```yaml
  governance_impact:
    outcome:
      classification: execution_halted
    report:
      - failure_reason
      - failed_test_list
  ```
- Can be used to plan evidence generation strategy

**Governance Readiness Principle:**
> Governance-related fields exist to support future governance utilization.

---

### Execution View

#### Concept
Defines interpretation viewpoint LLM should adopt, as interpretation hint not execution control.

#### Detail
**What It IS:**
- Perspective or lens (e.g., security, QA, architecture)
- Hint for interpretation emphasis

**What It IS NOT:**
- Responsibility assignment
- Role-playing instruction
- Human-in-the-loop declaration

**Relationship with Behavior Category:**
- Category: Behavior classification
- Execution View: Interpretation lens

**Core Purpose:**
- Reduces rule interpretation ambiguity
- Complementary to Decision Behavior Category without overlap

#### Example
```yaml
execution_view: security_engineer
```

---

### devSCR Architecture Core Principles

#### Concept
Structural representation of development-stage constraints describing AI decision behavior, not governance conclusions.

#### Detail
**Responsibility Segmentation:**
| Level | Responsibility |
|-------|----------------|
| DBG | Governance philosophy (What is governed) |
| DCL | Constraint architecture (Where governance exists) |
| devSCR | Structured representation of development-stage constraints (How, at dev time) |
| Runtime/Ops | Behavior observation, evidence, outcome marking |

**Key Principle:**
> devSCR describes "how AI must behave at decision time," not "what the governance conclusion of this behavior is."

**Execution Boundary Principle:**
> Execution = RNL × execution_role × applies_to

**Three Elements Entering Execution:**
1. RNL: Only layer AI agent can "understand as behavior requirement"
2. execution_role: AI's "duty role persona"
3. applies_to: Constraint scope limitation

---

### Struct Semantics Mechanism

#### Concept
Effectiveness comes from reducing degrees of freedom, not increasing command power.

#### Detail
**Core Concept:**
> LLM is not "commanded," but confined in a smaller semantic room.

**Struct Semantic Impact on LLM:**
| Struct Semantic | LLM Impact |
|-----------------|------------|
| WHY (intent) | Limits "what this is for" |
| WHO (roles) | Limits "who should obey" |
| WHAT (applies_to) | Limits "which output types are relevant" |
| WHEN (context) | Limits "under what circumstances relevant" |
| WHAT HAPPENS (effect) | Limits "semantic consequences of violation" |

**Why Particularly Effective for GPT-4.1+:**
> Model treats structure as "semantic boundary lines," not just data formats.

- YAML key ≈ semantic anchor
- Hierarchy ≈ priority / scope
- Capsule ≈ interpretation domain

---

### Validation Three-Layer Model

#### Concept
External tool responsibility, not BRA core, validating rule artifacts at three levels.

#### Detail
**Note:** Validation does not belong to BRA; it is an external tool layer responsibility.

**Layer 1: Artifact Structure Validation**
- Check YAML/JSON basic structure
- Use JSON Schema validator
- Result: pass / fail
- Does not touch RNL, does not touch semantics

**Layer 2: RNL Conformance Validation**
- Check if `rule.constraint` conforms to RNL representation schema
- Result: warning or fail (depending on mode)
- Not rule prerequisite, but quality gate
- Must be "configurable strict / advisory mode"

**Layer 3: Authoring Quality Advisory**
- Help human authors understand: field alignment, old/new format conversion, expression clarity
- Result: never should be failed, only produce warning / info
- Best position for AI as "auxiliary tool"

---

### Risk Potential (Three-Layer Risk Model)

#### Concept
Three-layer risk model with mandatory layer responsibility separation.

#### Detail
**Three-Layer Allocation:**
| Layer | Owner | Description |
|-------|-------|-------------|
| Risk Potential | BRA (Rule metadata) | A priori assessment: if something goes wrong, potential impact |
| Risk Signal | BCM | Runtime stage: risk sign detection |
| Realized Risk | Governance / Audit | Audit stage: actual consequences caused |

**Correct Usage (BRA):**
```yaml
risk_potential:
  impact_score: 4
  rationale: affects_release_safety
```

**Incorrect Usage:**
```yaml
risk: high
severity: high
```

**Resolving RSG.md vs devSCR Conflict:**
- RSG.md's `applicability_context` can include risk/severity as descriptive tags
- But should not serve as governance judgment
- devSCR's `risk_potential` is correct representation at design stage

---

### Attribute vs Imperative Separation

#### Concept
Strict separation between governance attributes (non-executable) and imperative statements (constrain AI behavior).

#### Detail
**Core Principle:**
> Only imperative statements constrain AI behavior. All attributes MUST be non-executable and governance-only.

**Correct (Attribute-based):**
```yaml
attributes:
  risk_potential:
    impact_score: 4
    rationale: affects_release_safety
  behavior_characteristics:
    autonomy_level: low
    requires_human_attention: true
```

**Incorrect (Governance Language):**
```yaml
risk: high
severity: critical
```

**Design Rationale:**
- Prevent AI from self-generating excessive defense or weight shifts due to seeing `risk: high`
- Maintain BRA's execution neutrality principle
- Ensure governance attributes not misread by AI as execution directives

---

### Key Quotes

#### Concept
Core insights expressing fundamental design philosophy.

#### Detail
**Architecture Level:**
> If it is injected, it is execution. If it is an attribute, it is governance.

> Governance does not depend on what is described, but on whether it is declarative or imperative.

> Rules fail not because models refuse them, but because models reinterpret them.

**Responsibility Boundary:**
> devSCR describes "how AI must behave at decision time," not "what the governance conclusion of this behavior is."

> Ruleset defines a decision rule lens, not a set of executable behaviors.

**Most Critical Statement:**
> BRA should only be responsible for "defining behavior constraints," not "executing, judging, governing, or validating."

**Core NNL Principle:**
> Important things should not be left for AI to guess.

**RNL Essence:**
> Not making AI freer, but making AI harder to deviate from boundaries.

---

### Rule as Governance Atom

#### Concept
Smallest unit carrying governance meaning with explicit behavioral requirement and context.

#### Detail
**Core Concept:**
> There is no governance without a rule. There is no rule without a policy or constraint.

**Single Rule Encapsulates:**
- One explicit behavioral policy (MUST) **or** constraint (MUST NOT)
- Its applicable scope
- Its governance interpretation context
- Its accountability and audit expectations

**Rule Mental Model:**
> A single constrained sentence, written so that AI has less room to misinterpret it, while humans and governance systems can still reason about it.

**Core Design Philosophy:**
- Rule is language artifact, designed for AI to read and interpret
- Remains suitable for governance, audit, and traceability
- Not governance process, not execution mechanism, not policy engine

---

### Common Misuse Patterns

#### Concept
Patterns to avoid when writing rules.

#### Detail
**Misuse Patterns:**
- Encoding IF/THEN logic
- Treating boundary as trigger
- Treating applicability_context as execution condition
- Embedding enforcement steps in governance_impact
- Using responsibilities to imply execution control

---

### Schema Metadata

#### Concept
Optional information that must not affect rule interpretation or execution.

#### Detail
**Decision:**
> optional + MUST NOT affect rule interpretation

**Recommended Specification:**
> Schema metadata MUST NOT be required for rule execution or interpretation.

**Recommended Approaches:**
- Approach A (recommended for examples/public docs): Move schema information to comments
- Approach B (for tools/pipelines): Keep `meta:`, but "tools can choose to ignore"

---

### RNL Specification

#### Concept
Light syntax specification without formal grammar.

#### Detail
**Decision:**
> Define "light syntax specification," do not do formal grammar

**Recommended Content:**
- MUST / MUST NOT only
- Forbidden vague words (e.g., maybe, try, possibly)
- Single behavior directive
- No nested conditions

**Implementation:**
- BRA defines RNL specification principles
- External Tools implement RNL parser / validator
- Tools can choose strict / advisory modes

---

### Evidence Generation

#### Concept
BRA enables evidence generation but does not define evidence structure.

#### Detail
**BRA's Role:**
> BRA enables evidence generation but does not define evidence structure.

**BCM's Role:**
- Define Evidence structure (GES - Governance Evidence Specification)
- Execute Evidence generation logic
- Manage Evidence storage and tracing

**Evidence Examples:**
- Execution Log
- Rule-evaluation Report
- Trace ID
- Change Summary
- Violation Evidence (Constraint violation)
- Compliance Evidence (Policy compliance)

---

### Consensus Decisions from AI Assistants

#### Concept
14 conflict points resolved through ChatGPT, Claude, Gemini, and Grok evaluation.

#### Detail
**Decision Overview:**

| ID | Topic | Decision | Priority | Responsible Layer |
|----|-------|----------|----------|-------------------|
| 2.1 | RNL Keywords | Only MUST / MUST NOT | High | BRA |
| 2.2 | execution_view | Keep (optional) | High | BRA |
| 2.3 | Risk Potential | Three-layer model (BRA uses risk_potential) | High | BRA / BCM / Gov |
| 2.4 | Attribute vs Imperative | Strict separation | High | BRA |
| 2.5 | Risk Trigger | Not in BRA (move to BCM) | Low | BCM |
| 2.6 | Rule Relations | Not in core (extension) | Low | Extension |
| 2.7 | Validation | Not BRA (external tool layer) | Low | External Tool |
| 2.8 | Schema Metadata | Optional + non-binding | Medium | BRA |
| 2.9 | Wording | Minimal adjustment (use x-meta) | Medium | BRA |
| 2.10 | Category vs Application | Strengthen three-layer iron law | High | BRA |
| 2.11 | Boundary | Two-layer (Rule declaration + external binding) | High | BRA / External |
| 2.12 | GES | Not in core (future work) | Low | Future Work |
| 2.13 | Ruleset awareness | Unidirectional reference (Ruleset → Rule) | Low | BRA |
| 2.14 | RNL/RNL specification | Define "light specification" (no formal grammar) | Medium | BRA |

**Architecture Purity Evaluation:**
After decisions, BRA becomes clean three-layer system:
```
Language Layer (NNL / RNL)
  ↓
Rule Architecture Layer (atomic rule)
  ↓
Governance Asset Layer (library / ruleset)
```

**Correctly Isolated:**
| Item | Destination |
|------|-------------|
| Risk trigger | BCM |
| Validation | Tooling |
| Evidence | GES |
| Execution | External |
| Boundary binding | External |

---

### Implementation Guidance

#### Concept
Practical guidance for updating concept.md and thesis writing.

#### Detail
**Immediate High Priority:**
1. Update Rule definition with Policy/Constraint classification
2. Update Ruleset definition emphasizing pure rule collection
3. Update overall hierarchy structure with two-layer Boundary
4. Add execution_view and risk_potential in Rule Architecture Layer
5. Update diagrams marking Policy/Constraint branches

**Medium Priority:**
- Use x-meta to annotate Attribute semantic roles
- Update examples showing Schema Metadata optional nature
- Define simplified RNL syntax principles

**Future Work/Low Priority:**
- Risk Trigger mechanism (BCM scope)
- Rule Relations (extension)
- Validation three-layer model (external tool layer)
- GES (Evidence structure)

**Thesis Writing Notes:**
1. Unify using "Rule" as top-level concept
2. Clearly distinguish Policy and Constraint, explaining their differences
3. Emphasize observability and enforceability differences: important practical discovery
4. Maintain BRA architecture purity: only responsible for defining constraints, not execution, validation, governance
5. Clear layering: BRA / BCM / External Tool responsibility boundaries must be clear

---

### Terminology Evolution

#### Concept
Mapping between early design terminology and current terminology.

#### Detail
**Terminology Mapping:**
| Early Term (in source file) | Current Term (concept.md) | Description |
|----------------------------|---------------------------|-------------|
| Constraint | Rule | Top-level concept |
| constraint (lowercase) | Policy / Constraint | Distinguish by MUST / MUST NOT |
| 約束 (Constraint) | 規則 (Rule) | Top-level concept |
| 單一約束 (Single constraint) | Policy or Constraint | Depending on MUST / MUST NOT |
| 約束獨占性 (Constraint exclusivity) | 規範性核心 (Normative core) | Both Policy and Constraint are normative core |

**Important Discovery:**
MUST and MUST NOT have fundamental practical differences in:
- Trigger observability
- Evidence generation
- Enforceability
- AI interpretation

This discovery led to explicit Policy/Constraint distinction in current design.

---

## IMPLICIT ASSUMPTIONS
- Structured representation reduces AI interpretation ambiguity
- Declarative approach superior to procedural for governance
- LLMs treat structure as semantic boundaries, not just data formats
- Governance and execution must be strictly separated
- Rule independence enables reusability across contexts

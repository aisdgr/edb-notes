AI governance has a boundary problem.

Most people say “boundary” — but they are talking about completely different things.

Let’s make it explicit.

There isn’t one boundary.
There are at least three.

Model Boundary
→ what the model can access or generate

Runtime Boundary
→ what actions are allowed to execute

Development Execution Task Boundary
→ what is even allowed to be decided

Most systems today focus here:

Runtime Boundary
ALLOW / DENY / HOLD
Execution gating
Approval workflows

That’s necessary.
But not sufficient.

Because something critical is missing:
If the decision space is not constrained,
invalid outcomes are still generated — just filtered later.

So the real question is not:
“How do we control execution?”
It is:
“How do we prevent invalid decisions from ever existing?”

This is where Scope comes in.

Scope defines what is governable:
Visible Scope
→ what the system can see
Expected Artifact Scope
→ what should exist
Decision Behavior Scope
→ how decisions are allowed to form
→ includes Policy and Constraint
Actual Effect Artifact Scope
→ what is actually produced

Key idea:

Policy and Constraint define the rules
Scope defines where they apply
Boundary defines whether they can happen

Governance = Scope × Boundary

If governance starts at runtime,
it is already too late.

I wrote more about this in my paper:
Boundary as an Execution-Time Primitive for AI-Assisted Software Development Governance

DOI: https://lnkd.in/g3ydtPcV
Preprint: https://lnkd.in/gHsr3PuR

Curious how others are defining “boundary” in AI systems.

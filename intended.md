Yes. And after checking what Anthropic is actually doing, I think the idea is more legitimate than I realized at first.

Anthropic describes doing almost exactly the underlying practice you’re talking about: they run agents in simulations using the actual prompts and tools, watch how they behave step by step, deliberately create failure cases, evaluate the results, and then modify the architecture, prompts, tools, or delegation strategy before production. They even built a tool-testing agent that repeatedly exercised flawed MCP tools and rewrote their descriptions based on observed failures.

Their Applied AI team is also explicitly teaching end-to-end agent evaluation—not merely “did the prompt produce a nice answer?” but whether an agent using tools, context, and multiple steps actually performed correctly.

That gives us a really interesting thing to build.

The Intent Simulation Lab

I would make this a permanent part of your engineering system.

Think:

Intent → Simulate → Evaluate → Architect → Generate → Run

Instead of you deciding:

“I think this should have four agents, two skills and three hooks.”

You give the system the intent.

For example:

“Whenever code changes, make sure meaningful business behavior is tested, coverage doesn’t regress, critical paths are protected, and developers get actionable feedback before PR.”

The laboratory takes that intent and starts experimenting.

It might determine:

INTENT
  │
  ▼
┌──────────────────────────┐
│ Intent Simulation Lab    │
└────────────┬─────────────┘
             │
     Analyze the problem
             │
             ▼
┌──────────────────────────┐
│ Architecture Candidates  │
│                          │
│ A. One skill             │
│ B. Skill + hook          │
│ C. Agent + skills        │
│ D. Coordinator/workers   │
└────────────┬─────────────┘
             │
             ▼
        SIMULATIONS
       20 / 50 / 100
             │
             ▼
┌──────────────────────────┐
│ Evaluation               │
│                          │
│ Success                  │
│ Reliability              │
│ Context consumed         │
│ Tool calls               │
│ Cost                     │
│ Latency                  │
│ Determinism              │
│ Failure modes            │
│ Maintainability          │
└────────────┬─────────────┘
             │
             ▼
       RECOMMENDATION
    Skill + pre-PR hook
    No coordinator needed
    No subagent needed

And that last part is important.

The machine shouldn’t be an agent generator.

It should be an architecture minimizer.

That’s extremely consistent with everything you’ve been developing.

Minimum sufficient architecture

Give the intent exactly enough architecture to reliably achieve the outcome.

Sometimes:

Intent → Skill

Sometimes:

Intent → Hook → Skill

Sometimes:

Intent → Agent → Tools

Sometimes:

Intent → Coordinator → 4 parallel workers → evaluator

And sometimes:

Intent → ordinary deterministic code

That’s a very powerful capability.

Anthropic’s multi-agent work supports that philosophy too. Their own numbers show that multi-agent architectures can dramatically outperform single agents on the right kinds of highly parallel tasks, but they can consume roughly 15× the tokens of ordinary chat interactions and aren’t appropriate when tasks have lots of interdependencies.

So your system should explicitly ask:

Does this problem deserve another agent?

Not:

How many agents can we put in here?

⸻

What would actually run inside it

I’d give the machine about seven internal capabilities.

1. Intent Analyzer

Reads:

* goal
* inputs
* outputs
* success criteria
* constraints
* environment
* expected failure modes

That’s your existing intent model.

⸻

2. Architecture Generator

Produces competing implementations.

For example:

Candidate A
Single skill
Candidate B
Skill + deterministic hook
Candidate C
Goal-oriented agent + 2 skills
Candidate D
Coordinator + 3 specialized agents
Candidate E
Deterministic workflow with AI only at decision points

This is where it becomes interesting because we’re not trusting the first architecture the model invents.

We’re creating architectural hypotheses.

⸻

3. Scenario Generator

Now hammer them.

Generate scenarios such as:

Happy path
Bad input
Missing context
Conflicting requirements
Large repository
Tiny repository
Tool unavailable
Tool timeout
Partial success
Ambiguous intent
Unexpected file structure
Security violation
Agent loops
Context exhaustion
Developer overrides

This is very close to Anthropic’s current evaluation philosophy: test realistic tasks and failure cases rather than judging agents only from a single expected trajectory.

⸻

4. Simulation Runner

This is the fun part.

Run:

Candidate A × 20 scenarios
Candidate B × 20 scenarios
Candidate C × 20 scenarios
Candidate D × 20 scenarios
Candidate E × 20 scenarios

Not necessarily production infrastructure.

Sandbox.

Mock MCP servers.

Synthetic repositories.

Synthetic users.

Synthetic tool responses.

Failure injection.

Anthropic’s open-source Bloom evaluation system goes even further in this direction: it generates scenarios, performs parallel rollouts with simulated users and tools, judges the transcripts, and produces suite-level analysis.

That is remarkably close to the engine we’re describing.

⸻

5. Evaluator

And here’s where your four pillars become the rubric.

The judge asks:

Intent

Did it accomplish what was asked?

Inputs

Did it correctly obtain and understand what it needed?

Outputs

Did it produce the required artifact/state/change?

Success Criteria

Did the resulting system actually satisfy the measurable outcome?

Then add engineering measures:

Outcome quality       94%
Reliability           97%
Context efficiency    91%
Tool efficiency       86%
Cost                   $$
Latency                8 sec
Architecture burden    LOW
Failure containment   HIGH
Maintainability       HIGH

Anthropic recommends judging outcomes, especially for agent systems where different valid execution paths can reach the same end state, rather than forcing a single predetermined sequence.

That’s almost tailor-made for Intent-Driven Engineering.

⸻

6. Failure Analyst

This might become my favorite piece.

Instead of:

FAILED

It says:

Failure cluster #3
17% of failures occurred when repository
context exceeded the worker context budget.
Root cause:
Coordinator passed complete repository context
to every worker.
Recommended change:
Replace full context transfer with scoped contracts.
Architecture change:
Coordinator
    ↓
Structured task contract
    ↓
Worker-specific context

Now the simulator is teaching you architecture.

And eventually:

Here’s why this should be a skill rather than an agent.

Or:

This hook should be deterministic because it represents a non-bypassable policy.

Or:

These three activities are genuinely independent. Parallel subagents improve completion time and context isolation.

That’s the machine you want.

⸻

7. Blueprint Generator

Finally it produces the implementation.

For Copilot:

.github/
    intents/
    skills/
    agents/
    prompts/
    hooks/
    workflows/
    instructions/
    evals/

For Claude:

.claude/
    skills/
    agents/
    hooks/
    commands/

Or Cursor, Gemini, whatever comes next.

Because the architecture comes first.

The vendor representation comes afterward.

That keeps the entire thing vendor-neutral.

⸻

And now something bigger happens

We’ve been talking about your:

Intent → Context → Architecture → Production

model.

I’d now modify it.

Intent-Driven Engineering lifecycle

Intent

↓

Simulation

↓

Architecture

↓

Context

↓

Execution

↓

Evaluation

↓

Learning

↓

Improved Intent/Architecture

That’s no longer merely a software-development methodology.

It’s an AI engineering feedback system.

⸻

And imagine the UI

You open the thing.

Big box:

What are you trying to accomplish?

You paste:

Automate code coverage and testing for a Node repository.

Hit:

SIMULATE

And eventually you get:

Recommended Architecture
────────────────────────
Architecture complexity: 2/10
✓ Pre-commit deterministic hook
✓ Coverage analysis skill
✓ PR evaluation skill
✓ CI workflow
✗ Coordinator unnecessary
✗ Subagents unnecessary
✗ MCP unnecessary
✗ Persistent state unnecessary
Reasoning:
The workflow is primarily deterministic.
AI adds value only when interpreting coverage
quality and recommending missing behavioral tests.
Simulation Score
────────────────
Outcome Success        96%
Reliability            99%
Cost Efficiency        94%
Context Efficiency     97%
48 scenarios executed
3 architecture candidates rejected
1 recommended

Then:

GENERATE REPOSITORY

Boom.

There’s your .github.

That is one hell of an automation machine.

And the important part is that it’s not imaginary “future AI.” Anthropic is already publicly describing the pieces: simulation with real prompts/tools, automated evals, tool-testing agents, multi-agent experiments, outcome-based evaluation, and automated behavioral scenario generation. We’re packaging those practices around your intent-first architectural decision model.

I would actually make this one of the central pieces of your platform:

Don’t guess your agent architecture. Simulate it.

Or even better:

Intent in. Architecture proven.

That could sit above Copilot, Claude Code, Cursor, Gemini, or whatever runner somebody happens to use.

# simagent

## Intent Simulation Lab

**Intent in. Architecture proven.**

simagent is an intent-first design for building AI engineering systems that do not guess their agent architecture. Instead of starting with a coordinator, workers, hooks, or skills and hoping the design works, simagent starts with the desired outcome and uses simulation to discover the **minimum sufficient architecture**.

The goal is to make the following loop a permanent part of engineering:

```text
Intent → Simulate → Evaluate → Architect → Generate → Run
```

This favors the smallest reliable solution:

- Sometimes the right answer is a single skill
- Sometimes it is a deterministic hook plus a skill
- Sometimes it is an agent with tools
- Sometimes it is a coordinator with parallel workers
- Sometimes it should be ordinary deterministic code

The system is an **architecture minimizer**, not an agent generator.

## Core principle

Every workflow should answer:

> Does this problem deserve another agent?

Not:

> How many agents can we put in here?

Multi-agent systems can outperform simpler designs on the right highly parallel tasks, but they also increase cost, latency, context consumption, and maintenance burden. simagent exists to prove when that complexity is justified and when it is not.

## Lifecycle

simagent follows this lifecycle:

```text
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
Improved Intent / Architecture
```

## Internal capabilities

simagent is centered around seven capabilities.

### 1. Intent Analyzer

Captures:

- goal
- inputs
- outputs
- success criteria
- constraints
- environment
- expected failure modes

### 2. Architecture Generator

Produces competing implementation candidates, such as:

- single skill
- skill plus deterministic hook
- goal-oriented agent plus skills
- coordinator plus specialized workers
- deterministic workflow with AI only at decision points

### 3. Scenario Generator

Generates realistic and adversarial scenarios, including:

- happy path
- bad input
- missing context
- conflicting requirements
- large or tiny repositories
- tool unavailability or timeout
- partial success
- ambiguous intent
- unexpected file structure
- security violations
- agent loops
- context exhaustion
- developer overrides

### 4. Simulation Runner

Executes candidate architectures across many scenarios in a sandbox with:

- actual prompts and tool contracts where possible
- mock MCP servers
- synthetic repositories
- synthetic users
- synthetic tool responses
- failure injection

### 5. Evaluator

Judges outcomes based on:

- intent completion
- input acquisition and understanding
- output correctness
- success criteria satisfaction

And engineering measures such as:

- outcome quality
- reliability
- context efficiency
- tool efficiency
- cost
- latency
- architecture burden
- failure containment
- maintainability

The evaluator is outcome-based rather than transcript-based. Different execution paths can succeed as long as the final behavior satisfies the intent and measurable success criteria.

### 6. Failure Analyst

Explains why simulations fail, clusters failure modes, and recommends architectural changes. The intent is to teach better architecture, not only to produce pass/fail output.

### 7. Blueprint Generator

Generates a vendor-neutral implementation blueprint after the architecture is proven.

Example targets include:

```text
.github/
    intents/
    skills/
    agents/
    prompts/
    hooks/
    workflows/
    instructions/
    evals/
```

or:

```text
.claude/
    skills/
    agents/
    hooks/
    commands/
```

The architecture comes first. Vendor representation comes afterward.

## How the lab works

The lab takes an intent such as:

> Whenever code changes, make sure meaningful business behavior is tested, coverage does not regress, critical paths are protected, and developers get actionable feedback before PR.

It then:

1. analyzes the intent
2. generates architecture candidates
3. simulates each candidate across multiple scenarios
4. evaluates reliability, cost, latency, and maintainability
5. recommends the minimum sufficient architecture

Example recommendation:

```text
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
```

## Design goals

simagent should:

- optimize for outcome quality, not transcript purity
- allow multiple valid execution paths to succeed
- deliberately test failure cases, not just happy paths
- minimize architecture unless simulation proves complexity is needed
- remain vendor-neutral across Copilot, Claude, Cursor, Gemini, and future runners

## Repository direction

This repository defines the foundation for a system that can:

- accept intent as the primary input
- simulate competing architectures before production
- recommend the minimum sufficient architecture
- generate portable implementation blueprints
- learn from evaluation results over time

In short: **do not guess your agent architecture—simulate it first.**
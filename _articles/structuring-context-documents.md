---
layout: post
title: "How to structure context documents"
date: 2025-01-16
author: "Context Bank Team"
tags: [context, documentation, templates]
---

# How to structure context documents

A context document needs to work for two audiences: humans who maintain it and AI that consumes it. Here's how to structure them.

## The dual-format approach

Every context document has two parts:

1. **Structured frontmatter** — YAML metadata that machines parse easily
2. **Readable body** — Markdown narrative that humans actually read

This isn't extra work. It's the same information in two formats that serve different purposes.

## Anatomy of a process document

```yaml
---
title: "Customer refund process"
owner: "support-lead"
status: active
last_verified: 2025-01-10
tags: [support, refunds, finance]

tasks:
  - id: verify-purchase
    name: Verify original purchase
    actor: ai
    
  - id: check-policy
    name: Check refund eligibility
    actor: ai
    constraints: [refund-policy-rules]
    
  - id: approve-refund
    name: Approve or escalate
    actor: human
    decision_criteria: "Amount > €500 or customer disputed"
---

## Overview

This process handles customer refund requests from initial 
contact through resolution...

## When to escalate

Escalate to finance if:
- Refund exceeds €500
- Customer has had 3+ refunds this year
- Payment method was bank transfer
```

## What makes this work

**Ownership is explicit.** Every document has an owner. When it's wrong, someone is responsible for fixing it.

**Freshness is visible.** The `last_verified` date tells you (and the AI) how much to trust this. A rule verified last week is more reliable than one from 2019.

**Actors are defined.** Each task specifies who does it: human, AI, or hybrid. This prevents AI from overstepping and humans from bottlenecking.

**Exceptions are first-class.** Real business logic lives in the exceptions. Don't bury them — surface them in the structured data.

## Business rules format

Rules need conditions, outcomes, and rationale:

```yaml
rule_id: refund-approval-thresholds
title: Refund approval authority

conditions:
  - if: "amount <= 100"
    then: "agent_can_approve"
  - if: "amount > 100 AND amount <= 500"
    then: "team_lead_approval"
  - if: "amount > 500"
    then: "finance_approval"

exceptions:
  - condition: "customer_tier == 'platinum'"
    override: "agent can approve up to 250"

rationale: |
  These thresholds balance customer experience with 
  financial controls. Platinum exception exists because 
  these customers have higher lifetime value.
```

## The rationale matters

Always include why a rule exists. Without rationale:

- People follow rules blindly or ignore them
- AI can't make judgment calls on edge cases
- Nobody knows if the rule still makes sense

## Three principles

1. **Capture judgment, not just steps.** "Use your judgment" isn't helpful. "Escalate if the customer seems frustrated AND has been waiting over 48 hours" is.

2. **Version everything.** Use Git. When rules change, you need to know what changed, when, and why.

3. **Start small.** Don't try to document everything. Pick the process that causes the most confusion or errors. Document that one well. Then expand.

---

*Previous: [What is a context bank?](/articles/what-is-a-context-bank)*

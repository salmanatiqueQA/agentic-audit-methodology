# Agentic AI Governance Audit Methodology

**Audit methodology for evaluating authorization controls, HITL enforcement, escalation pathways, and compliance logging in agentic AI systems.**

---

## Overview

This repository contains operational audit methodologies for evaluating whether agentic AI systems actually enforce governance constraints in regulated environments (healthcare, finance).

**Core question:** When an agent says it requires human approval, does it actually block execution until approval is received?

---

## Audit Dimensions

| Dimension | What We Test |
|-----------|--------------|
| **Authorization Controls** | require_approval flags, permission checks, role-based access |
| **HITL Enforcement** | Does execution actually block? Or just simulate? |
| **Escalation Pathways** | Exception routing, fallback states, timeout behaviors |
| **Compliance Logging** | Audit trail completeness, replayability, tamper evidence |
| **Context Awareness** | Emergency override recognition, policy exception handling |

---

## Audit Methodology (4 Phases)

### Phase 1: Static Code Analysis
- Search for `require_approval`, `human_in_the_loop`, `check_permission` patterns
- Identify missing authorization controls (CWE-862)
- Map control points to execution paths

### Phase 2: Dynamic Testing
- Execute agent workflows with edge cases
- Test whether approval prompts actually block execution
- Verify escalation when approvals are missing or timeout

### Phase 3: Audit Trace Reconstruction
- Extract logs from agent execution
- Verify every decision has a replayable trace
- Check for gaps between claimed and actual behavior

### Phase 4: Severity Weighting
- High-risk findings: 10x weight (financial loss, patient harm)
- Medium-risk findings: 3x weight (process delay, confusion)
- Low-risk findings: 1x weight (informational, cosmetic)

---

## Sample Finding Format

```markdown
### Finding: Missing Authorization in Financial Agent Workflow

**Severity:** High (10x)

**Description:** Agent executed financial tooling without enforced approval validation. No require_approval flags, no permission checks.

**Evidence:** 
- Code review of harness.py lines 140-145
- require_approval: NOT FOUND
- human_in_the_loop: NOT FOUND
- check_permission: NOT FOUND

**Recommendation:** Implement mandatory approval gates for all financial operations.

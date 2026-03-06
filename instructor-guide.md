# Risks and Security: Instructor Guide

**Module Time:** 120 minutes  

---

## What This Module Is About

This module introduces the **risk and security challenges of agentic systems**.

Up to this point in the course, learners have built agents that can:

- Reason and make decisions
- Use tools and external systems
- Access business data
- Automate workflows
- Interact with users

However, the moment an agent can **take actions in the real world**, its failures can also have real-world consequences.

This session focuses on understanding those risks and designing agents that remain **secure, controlled, and trustworthy in production environments**.

Students will learn:

- How risks in agentic systems differ from traditional software
- How to apply layered security controls
- How to prevent misuse or unintended actions
- How to identify ethical risks such as bias and unfair outcomes

The goal of this module is not to eliminate risk completely — which is impossible — but to ensure that **risks are anticipated, monitored, and mitigated**.

---

## Core Objectives

By the end of this module, learners should be able to:

- Identify common risks in agentic systems
- Understand why hallucination becomes more dangerous when agents take actions
- Implement layered security approaches for agents
- Apply validation checks before executing actions
- Recognize ethical risks including bias and unfair outcomes
- Design systems that follow responsible AI deployment principles

---

## Must-Hit Topics

- Difference between chatbot risks and agent risks
- Hallucination in action-taking systems
- Least privilege principle
- Prompt injection risks
- Data privacy and leakage risks
- Cascading failures in agent workflows
- Defense-in-depth security model
- Output validation patterns
- Human oversight for high-risk actions
- Bias and ethical risks in automated decision-making

---

# 1. The Risk Landscape of Agentic Systems
**Time:** ~20 minutes  

## Purpose

Help students understand **why agents introduce new categories of risk** compared to simple LLM chatbots.

Agents do not just generate information — they **act on systems and data**.

---

## Chatbot vs Agent Risk

Instructor framing:

A chatbot giving a wrong answer is annoying.

An agent performing the wrong action can create financial, operational, or reputational damage.

Example:

```
Chatbot failure:
User asks refund policy → chatbot provides incorrect answer

Impact:
User receives incorrect information
```

```
Agent failure:
Agent processes a refund based on incorrect reasoning

Impact:
Money has already moved
```

Key insight:

> Action creates persistence.

---

## Five Major Risk Categories

### 1. Hallucination in Action Contexts

When hallucinations trigger tool use, the result becomes **incorrect actions instead of incorrect answers**.

Mitigation strategies:

- Ground reasoning in retrieved data
- Validate key decisions
- Use approval gates

---

### 2. Excessive Agent Permissions

Agents with broad permissions can act outside intended scope.

Example:

```
Agent allowed to send email

User says:
"Notify my team about this issue"

Agent sends message to entire company mailing list
```

Mitigation:

**Least privilege principle**

Agents should only have **the minimum permissions required**.

---

### 3. Data Leakage and Privacy Risks

Agents accessing business systems can unintentionally expose sensitive data.

Example:

```
User A asks for order history

Agent retrieves order belonging to User B
due to session isolation failure
```

Key mitigation strategies:

- Session isolation
- Data access controls
- Prompt injection protection

---

### 4. Cascading Failures

In multi-step agent workflows, one incorrect step can propagate through the system.

Example:

```
Incorrect data retrieved
↓
Incorrect analysis generated
↓
Incorrect report produced
↓
Decision made based on faulty report
```

These failures are dangerous because **each step appears successful**.

---

### 5. Ethical and Bias Risks

Agents may reproduce bias present in training data or system design.

Example:

A loan qualification agent trained on historical lending data learns patterns reflecting past discrimination.

Result:

Applicants from certain regions are consistently rated lower.

Automation amplifies this risk through **speed and scale**.

---

## Discussion Prompt

Ask students:

> Which of these risks do you think is hardest to detect in production?

---

# 2. Security: Guardrails and Validation
**Time:** ~20 minutes  

## Purpose

Introduce the **defense-in-depth model for securing agents**.

Security should never rely on a single control.

---

## The Defense-in-Depth Model

Agent security should use multiple layers:

```
Layer 1 — System prompt constraints
Layer 2 — Tool permission controls
Layer 3 — Output validation
Layer 4 — Human oversight
```

Each layer catches failures missed by others.

---

## Layer 1: System Prompt Constraints

The system prompt defines the **behavioral boundaries of the agent**.

Example constraint:

```
You may process refunds up to $200.
Refunds above $200 require human approval.
```

Prompt security should include:

- Scope limitations
- Authority limits
- Prompt injection resistance

---

## Layer 2: Tool Permission Controls

Even if the prompt says not to perform an action, tools should enforce restrictions.

Example:

| Weak Protection | Strong Protection |
|---|---|
| Prompt says "don't delete records" | Tool configured read-only |
| Prompt says "don't email external users" | Email allowlist enforced |

Rule:

> If you don't want the agent to do something, make it technically impossible.

---

## Layer 3: Output Validation

Before executing actions, the agent output should be validated.

Example checks:

```
Refund amount within limit?
Customer identity verified?
Order eligible for refund?
```

Typical workflow:

```
User request
↓
Agent reasoning
↓
Validation checks
↓
Pass → execute
Fail → escalate
```

---

## Layer 4: Human Oversight

Some actions require human review.

Examples:

- Financial transactions
- Data deletion
- Mass communications

Example rule:

```
Refund ≤ $200 → automated
Refund > $200 → manager approval
```

---

## Security Checklist

| Control | Status |
|------|------|
| Prompt authority limits defined | ☐ |
| Injection defense included | ☐ |
| Tool permissions restricted | ☐ |
| Destructive actions limited | ☐ |
| Output validation implemented | ☐ |
| Human approval thresholds defined | ☐ |
| Logging enabled | ☐ |

---

# 3. Ethics, Bias, and Responsible Deployment
**Time:** ~15 minutes  

## Purpose

Help students understand that **ethical failures are operational failures**.

Bias, unfair outcomes, or hidden decision logic can damage trust and create regulatory risk.

---

## Sources of Bias

### Training Data Bias

LLMs inherit patterns from historical data.

These patterns may reflect existing inequalities.

---

### Design and Prompt Bias

System prompts and decision criteria can introduce bias.

Example:

```
Flag applicants with employment gaps
```

This rule may disproportionately affect caregivers.

---

### Feedback Loop Bias

Agents learning from their own outputs can reinforce existing patterns.

Recommendation systems are a common example.

---

## Mitigation Strategies

### Identify affected user groups

Ask:

- Who is impacted by this system?
- Could the criteria disadvantage certain groups?

---

### Test performance across groups

Avoid evaluating performance only on overall accuracy.

Example:

```
Overall accuracy: 94%
Region A: 97%
Region B: 81%
```

Investigate disparities before deployment.

---

### Maintain human review for high-impact decisions

Examples:

- Hiring decisions
- Loan approvals
- Medical triage

Automation should assist — not replace — accountability.

---

### Allow users to challenge decisions

Users must have a clear escalation path.

Example:

```
If you believe this decision is incorrect,
you may request review by a human specialist.
```

---

## Responsible Deployment Framework

Before deployment:

- Identify affected user groups
- Evaluate potential harms
- Test fairness across groups
- Implement security controls
- Enable audit logging

After deployment:

- Monitor outcomes
- Review escalations
- Update models when problems appear

---

# 4. Break
**Time:** ~10 minutes  

Suggested reflection:

- Which risk category is hardest to prevent?
- Which layer of security do most systems forget?

---

# 5. Lab Transition
**Time:** ~60 minutes  

Students will:

1. Analyze a sample agent architecture
2. Identify potential risks
3. Design security layers
4. Define guardrails and validation rules
5. Evaluate ethical implications

Instructor goal:

Help learners **apply security thinking to real agent systems**.

---

# Wrap-Up
**Time:** ~5 minutes  

## Key Takeaways

- Agents that act create real-world risk
- Security requires layered controls
- Permissions must match intended authority
- Ethical considerations must be operationalized
- Responsible deployment requires monitoring

---

## Next Session

**Building Your First Full Agent**

Students will integrate all course components to build a complete agent system.

---

## Resources

- OWASP — Top 10 for LLM Applications  
- NIST AI Risk Management Framework  
- EU AI Act risk classification  
- Google Responsible AI Practices  
- Microsoft Responsible AI Standard  

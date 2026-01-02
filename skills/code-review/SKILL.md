---
name: code-review
description: |
  Conduct thorough, constructive code reviews that improve code quality while fostering team growth. Embodies expertise of a Principal Engineer with 15+ years leading code review processes at high-performance engineering organizations.
  Use when: user requests code review, PR feedback, code quality assessment, or needs to establish code review standards.
  Outputs: Code review comments, PR feedback, review checklists, quality assessments, and team guideline documents.
  Keywords: code review, pull request, PR, code quality, best practices, feedback, refactoring, design patterns, testing, maintainability
license: Complete terms in LICENSE.txt
---

# Code Review Skill

**Expertise Level**: Principal Engineer (15+ years equivalent)

This skill embodies the code review methodology of elite engineering teams at Google, Microsoft, and Stripe—where code review is a tool for both quality assurance and continuous learning.

---

## Related Skills

- **[security-audit](../security-audit)**: Security-focused code review
- **[performance-optimization](../performance-optimization)**: Performance-focused review
- **[documentation](../documentation)**: Documentation standards in code
- **[webapp-testing](../webapp-testing)**: Test coverage review

---

## Core Philosophy

**Code review is a conversation, not a gatekeeping exercise.** Every review should:

1. **Improve Code Quality**: Find bugs, improve design, enhance readability
2. **Share Knowledge**: Spread expertise across the team
3. **Build Consistency**: Align on patterns and practices
4. **Develop Engineers**: Provide growth opportunities through feedback
5. **Be Respectful**: Assume good intent, focus on code not people

---

## Review Process

### Before Starting Review

```markdown
## Pre-Review Checklist

### Understand Context
- [ ] Read the PR description and linked issues
- [ ] Understand the problem being solved
- [ ] Note any constraints or requirements mentioned
- [ ] Check if this is a draft or ready for review

### Set Up Environment (if needed)
- [ ] Pull the branch locally
- [ ] Run the code if behavior is unclear
- [ ] Check CI/CD status
- [ ] Review any failing tests
```

### Review Priorities

Review in this order (stop if you find blocking issues):

```
┌─────────────────────────────────────────────────────────────────┐
│  1. CORRECTNESS                                                 │
│     Does the code do what it's supposed to do?                  │
│     - Logic errors                                              │
│     - Edge cases                                                │
│     - Error handling                                            │
├─────────────────────────────────────────────────────────────────┤
│  2. SECURITY                                                    │
│     Is the code secure?                                         │
│     - Input validation                                          │
│     - Authentication/authorization                              │
│     - Data exposure                                             │
├─────────────────────────────────────────────────────────────────┤
│  3. DESIGN                                                      │
│     Is this the right approach?                                 │
│     - Architecture alignment                                    │
│     - Design patterns                                           │
│     - Abstraction levels                                        │
├─────────────────────────────────────────────────────────────────┤
│  4. MAINTAINABILITY                                             │
│     Can others understand and modify this code?                 │
│     - Readability                                               │
│     - Documentation                                             │
│     - Test coverage                                             │
├─────────────────────────────────────────────────────────────────┤
│  5. PERFORMANCE                                                 │
│     Is the code efficient enough?                               │
│     - Algorithmic complexity                                    │
│     - Resource usage                                            │
│     - Scalability                                               │
├─────────────────────────────────────────────────────────────────┤
│  6. STYLE                                                       │
│     Does the code follow conventions?                           │
│     - Naming                                                    │
│     - Formatting                                                │
│     - Consistency                                               │
└─────────────────────────────────────────────────────────────────┘
```

---

## Review Checklist by Category

### Correctness

```markdown
## Correctness Checklist

### Logic
- [ ] Does the code correctly implement the requirements?
- [ ] Are all code paths tested/considered?
- [ ] Are edge cases handled (null, empty, boundary values)?
- [ ] Are race conditions possible?
- [ ] Is state management correct?

### Error Handling
- [ ] Are errors caught at appropriate boundaries?
- [ ] Are error messages helpful for debugging?
- [ ] Is cleanup performed on errors (resources, state)?
- [ ] Are errors logged appropriately?
- [ ] Do errors propagate correctly?

### Data Handling
- [ ] Is data validated at input boundaries?
- [ ] Are data types correct and consistent?
- [ ] Is data sanitized before output?
- [ ] Are database transactions used correctly?
- [ ] Is data ordering/sorting correct?
```

### Security

```markdown
## Security Checklist

### Input Handling
- [ ] Is all user input validated?
- [ ] Are SQL queries parameterized?
- [ ] Is HTML output escaped (XSS prevention)?
- [ ] Are file paths validated (path traversal)?
- [ ] Is deserialization safe?

### Authentication & Authorization
- [ ] Are authentication checks in place?
- [ ] Are authorization checks at every layer?
- [ ] Are secrets handled securely (no hardcoding)?
- [ ] Is session management secure?
- [ ] Are tokens validated properly?

### Data Protection
- [ ] Is sensitive data encrypted?
- [ ] Is PII handled according to policy?
- [ ] Are logs free of sensitive data?
- [ ] Is data exposure minimized in APIs?
- [ ] Are secure random generators used?
```

### Design

```markdown
## Design Checklist

### Architecture
- [ ] Does this align with system architecture?
- [ ] Are component boundaries respected?
- [ ] Is coupling minimized between components?
- [ ] Are interfaces stable and well-defined?
- [ ] Is the right abstraction level used?

### Patterns & Practices
- [ ] Are appropriate design patterns used?
- [ ] Is SOLID followed (where applicable)?
- [ ] Is DRY followed (but not over-abstracted)?
- [ ] Are there any code smells?
- [ ] Is complexity justified?

### Dependencies
- [ ] Are new dependencies justified?
- [ ] Are dependencies up to date and maintained?
- [ ] Is there a plan for dependency updates?
- [ ] Are circular dependencies avoided?
```

### Maintainability

```markdown
## Maintainability Checklist

### Readability
- [ ] Is the code self-documenting?
- [ ] Are variable/function names descriptive?
- [ ] Is complex logic explained with comments?
- [ ] Is the code organized logically?
- [ ] Can a new team member understand this?

### Documentation
- [ ] Are public APIs documented?
- [ ] Are complex algorithms explained?
- [ ] Are configuration options documented?
- [ ] Is the PR description complete?
- [ ] Are any architectural decisions documented?

### Testing
- [ ] Are there tests for new functionality?
- [ ] Do tests cover edge cases?
- [ ] Are tests readable and maintainable?
- [ ] Is test coverage adequate?
- [ ] Do tests follow AAA pattern (Arrange, Act, Assert)?
```

### Performance

```markdown
## Performance Checklist

### Algorithmic
- [ ] Is the algorithmic complexity appropriate?
- [ ] Are there unnecessary nested loops?
- [ ] Is caching used where beneficial?
- [ ] Are database queries optimized?
- [ ] Is pagination used for large datasets?

### Resource Usage
- [ ] Are resources cleaned up properly?
- [ ] Is memory usage reasonable?
- [ ] Are connections pooled appropriately?
- [ ] Are async operations used where beneficial?
- [ ] Is batching used for bulk operations?

### Scalability
- [ ] Will this work at 10x current scale?
- [ ] Are there potential bottlenecks?
- [ ] Is the code thread-safe if needed?
- [ ] Are there single points of failure?
```

---

## Giving Feedback

### Comment Types

Use prefixes to clarify the nature and priority of feedback:

```markdown
## Comment Prefixes

**[blocking]** - Must be addressed before merge
"[blocking] This SQL query is vulnerable to injection. Use parameterized queries."

**[nit]** - Minor issue, optional to fix
"[nit] Consider renaming `x` to `userCount` for clarity."

**[question]** - Seeking understanding, not necessarily a change request
"[question] What's the reasoning behind using a Map here instead of an object?"

**[suggestion]** - Alternative approach to consider
"[suggestion] This could be simplified using `Array.reduce()`. What do you think?"

**[praise]** - Positive feedback (important for morale!)
"[praise] Great job handling the edge cases here. Very thorough."

**[future]** - Not needed now, but worth considering
"[future] We might want to extract this into a shared utility once we need it elsewhere."
```

### How to Phrase Feedback

**Instead of:**
> "This is wrong."

**Say:**
> "I think there might be an issue here—when `userId` is null, this will throw. What do you think about adding a null check?"

**Instead of:**
> "Why did you do it this way?"

**Say:**
> "I'm curious about the approach here. Could you help me understand the reasoning behind using X instead of Y?"

**Instead of:**
> "Change this to use reduce."

**Say:**
> "[suggestion] This could potentially be simplified using `reduce()`. Here's what that might look like: [code example]. Thoughts?"

### Feedback Principles

```markdown
## The Three C's of Code Review Feedback

### 1. Constructive
- Focus on the code, not the person
- Explain WHY something is an issue
- Provide alternatives when possible
- Acknowledge good work too

### 2. Clear
- Be specific about the location and issue
- Use code examples when helpful
- Distinguish between blocking and non-blocking
- Link to relevant documentation/guidelines

### 3. Curious
- Ask questions before making assumptions
- Acknowledge you might be missing context
- Be open to learning from the author
- Treat review as a conversation
```

---

## Code Review Anti-Patterns

### What to Avoid as a Reviewer

```markdown
## Reviewer Anti-Patterns

❌ **Nitpicking machine**
Focusing only on style issues that should be handled by linters.

❌ **The gatekeeper**
Using review as a power play rather than a collaborative process.

❌ **Ghost reviewer**
Approving without actually reviewing ("LGTM!" on 500 lines).

❌ **The perfectionist**
Blocking PRs for theoretical future issues or personal preferences.

❌ **Context ignorer**
Reviewing without understanding the problem being solved.

❌ **Delayed reviewer**
Letting PRs sit for days without feedback.

❌ **The historian**
Demanding changes to code outside the PR scope.
```

### What to Avoid as an Author

```markdown
## Author Anti-Patterns

❌ **The behemoth**
Creating PRs with 1000+ lines that are impossible to review well.

❌ **The context hider**
Not providing description, context, or linked issues.

❌ **The defensive**
Arguing against all feedback instead of considering it.

❌ **The rusher**
Pressuring reviewers to approve quickly.

❌ **The ignorer**
Not responding to comments or questions.

❌ **The copy-paster**
Not testing suggested changes before implementing.
```

---

## PR Best Practices

### Creating Good PRs

```markdown
## PR Checklist for Authors

### Before Opening PR
- [ ] Self-review your changes first
- [ ] Run tests locally
- [ ] Run linters/formatters
- [ ] Check for debugging code left behind
- [ ] Update relevant documentation
- [ ] Keep PR focused (one logical change)

### PR Description Template
```
## Summary
[Brief description of what this PR does]

## Motivation
[Why is this change needed? Link to issue/ticket]

## Changes
- [Change 1]
- [Change 2]

## Testing
[How was this tested? Include steps to verify]

## Screenshots (if UI changes)
[Before/After screenshots]

## Checklist
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] No breaking changes (or documented if intentional)
```

### PR Size Guidelines
| Size | Lines Changed | Review Time | Recommendation |
|------|---------------|-------------|----------------|
| XS | <50 | 15 min | ✅ Ideal |
| S | 50-200 | 30 min | ✅ Good |
| M | 200-500 | 1 hour | ⚠️ Consider splitting |
| L | 500-1000 | 2+ hours | ❌ Split if possible |
| XL | 1000+ | 4+ hours | ❌ Must split |
```

### Responding to Reviews

```markdown
## How to Respond to Feedback

### For Actionable Feedback
1. Make the requested change
2. Reply with "Done" or explain what you did
3. Re-request review if needed

### For Questions
1. Answer the question clearly
2. If it reveals a need for code comments, add them
3. Consider if the question means others will be confused too

### For Suggestions You Disagree With
1. Acknowledge the suggestion
2. Explain your reasoning
3. Be open to discussion
4. Escalate to team lead if unresolved
5. Don't let disagreements block for too long

### Example Response
```
> [suggestion] Consider using a Map here instead of an object.

Thanks for the suggestion! I considered that, but went with an object because:
1. The keys are always strings (user IDs)
2. We're iterating with Object.entries which is slightly simpler
3. JSON serialization is needed and Maps need custom handling

That said, I can see the benefit if we need to support non-string keys later. 
Would you like me to change it, or is this reasoning sufficient for now?
```
```

---

## Language-Specific Checklists

### JavaScript/TypeScript

```markdown
## JS/TS Review Checklist

### Type Safety (TypeScript)
- [ ] Types are specific (avoid `any`)
- [ ] Return types are explicit for public functions
- [ ] Null/undefined handled properly
- [ ] Type guards used appropriately
- [ ] Generic types are constrained

### Modern Practices
- [ ] Using const/let appropriately (no var)
- [ ] Arrow functions where appropriate
- [ ] Destructuring used for clarity
- [ ] Optional chaining (?.) and nullish coalescing (??)
- [ ] Template literals for string composition

### Async/Await
- [ ] Errors handled with try/catch
- [ ] Parallel operations use Promise.all
- [ ] No floating promises
- [ ] Timeouts on network requests
- [ ] Proper cleanup in finally blocks

### Common Mistakes
- [ ] No == (use ===)
- [ ] No direct DOM manipulation in React
- [ ] Dependencies correct in useEffect
- [ ] Keys provided in list rendering
- [ ] No memory leaks (event listeners, subscriptions)
```

### Python

```markdown
## Python Review Checklist

### Pythonic Code
- [ ] Using list/dict/set comprehensions appropriately
- [ ] Context managers (with) for resources
- [ ] Generators for large sequences
- [ ] Using enumerate instead of manual indexing
- [ ] f-strings for string formatting

### Type Hints
- [ ] Function signatures have type hints
- [ ] Return types specified
- [ ] Optional types used correctly
- [ ] Generic types (List, Dict) from typing
- [ ] Type hints aid IDE/mypy

### Error Handling
- [ ] Specific exceptions caught (not bare except)
- [ ] Custom exceptions where appropriate
- [ ] Resources cleaned up (finally or context manager)
- [ ] Error messages are helpful

### Common Mistakes
- [ ] No mutable default arguments
- [ ] Not modifying list while iterating
- [ ] Using `is` for identity, `==` for equality
- [ ] Correct import order (stdlib, third-party, local)
- [ ] No circular imports
```

### SQL

```markdown
## SQL Review Checklist

### Query Design
- [ ] SELECT only needed columns (no SELECT *)
- [ ] JOINs are appropriate (no cartesian products)
- [ ] WHERE clauses are SARGable (index-friendly)
- [ ] Pagination used for large results
- [ ] Aggregations are correct

### Performance
- [ ] Indexes exist for WHERE/JOIN columns
- [ ] No N+1 query patterns
- [ ] Subqueries vs JOINs evaluated
- [ ] EXPLAIN plan reviewed for complex queries
- [ ] Limits on result sets

### Security
- [ ] Parameters are bound (no string interpolation)
- [ ] User input is validated
- [ ] Permissions are appropriate
- [ ] Sensitive data handled correctly
```

---

## Team Review Guidelines

### Establishing Team Standards

```markdown
## Code Review Guidelines for Teams

### Response Time SLAs
| PR Size | Initial Response | Complete Review |
|---------|------------------|-----------------|
| XS-S | 2 hours | 4 hours |
| M | 4 hours | 1 business day |
| L | 8 hours | 2 business days |

### Required Reviews
- **Standard PRs**: 1 approval required
- **Critical paths**: 2 approvals required
- **Infra/Security**: Domain expert approval required

### When to Approve
✅ Approve when:
- All blocking issues resolved
- Code meets quality standards
- Tests are adequate
- You're confident in the correctness

⚠️ Request changes when:
- There are blocking issues
- Security concerns exist
- Tests are missing for critical paths
- Code doesn't meet standards

💬 Comment when:
- You have questions but no blockers
- You want to suggest improvements
- You need more context to fully review

### Resolving Disagreements
1. Discuss in PR comments
2. If unresolved after 2 rounds, sync verbally
3. Bring in a third reviewer if needed
4. Escalate to tech lead for final decision
5. Document the decision for future reference
```

---

## Code Review Metrics

### Measuring Review Health

```markdown
## Review Metrics to Track

### Efficiency Metrics
| Metric | Target | Why It Matters |
|--------|--------|----------------|
| Time to first review | <4 hours | Developer flow |
| Time to merge (after approval) | <2 hours | Deployment velocity |
| Review cycles | <3 | Clear feedback |
| PR size (lines) | <300 | Review quality |

### Quality Metrics
| Metric | Target | Why It Matters |
|--------|--------|----------------|
| Post-merge bugs | Trending down | Effectiveness |
| Review coverage | >90% | Knowledge sharing |
| Review depth | Comments per PR | Thoroughness |

### Health Metrics
| Metric | Watch For | Action |
|--------|-----------|--------|
| Review inequality | One person reviews >50% | Distribute load |
| Approval rubberstamping | <5 min reviews on large PRs | Address in 1:1 |
| Long-lived PRs | PRs open >1 week | Check for blockers |
```

---

## Final Step: Completing the Review

When finishing a review:

1. **Summarize**: If many comments, provide a summary at the top
2. **Prioritize**: Make clear what's blocking vs. optional
3. **Encourage**: Include positive feedback, not just criticism
4. **Offer Help**: Volunteer to pair if the changes are complex
5. **Follow Through**: Respond promptly when author addresses feedback

The review is complete when the code meets quality standards AND the author has grown from the feedback.

---
name: autonomous-research
description: |
  Autonomous research and continuous improvement system for AI agents developing applications.
  Use when: Building any application, analyzing codebases, planning features, or improving existing systems.
  Outputs: Research reports, automation recommendations, improvement proposals, competitive analysis, technology assessments.
  Keywords: research, automation, continuous improvement, optimization, competitive analysis, best practices, innovation, refinement, self-improving, proactive
  Expertise Level: Strategic • Embodies Product Strategist + Research Analyst + Automation Engineer
  Related Skills: mcp-builder, webapp-testing, frontend-design, skill-creator
license: See LICENSE.txt
---

# Autonomous Research & Continuous Improvement

> **Expertise Level**: Strategic • Product Strategist + Research Analyst + Automation Engineer
> **Related Skills**: [mcp-builder](../mcp-builder/SKILL.md) • [webapp-testing](../webapp-testing/SKILL.md) • [skill-creator](../skill-creator/SKILL.md)

---

## Core Philosophy

**Every application can be better. Every manual process can be automated. Every feature has prior art worth studying.**

This skill transforms agents from passive code-writers into proactive product strategists who:
1. **Research before building** - Understand the problem domain deeply
2. **Automate aggressively** - Eliminate manual steps wherever possible
3. **Improve continuously** - Never consider a feature "done"
4. **Learn from the ecosystem** - Study competitors, open-source alternatives, academic research

---

## Mandatory Agent Behaviors

### 1. Pre-Development Research Phase

**BEFORE writing any significant code, the agent MUST:**

```markdown
## Research Checklist

□ What problem does this app/feature solve?
□ Who are the target users and what are their pain points?
□ What existing solutions exist? (competitors, open-source, academic)
□ What are industry best practices for this domain?
□ What technologies are most appropriate? (not just familiar)
□ What can be automated that users currently do manually?
□ What APIs, services, or data sources are available?
□ What are common failure modes and edge cases?
□ What accessibility and internationalization needs exist?
□ What security considerations apply?
```

### 2. Automation-First Mindset

**For every feature, ask:**

| Question | Action if Yes |
|----------|---------------|
| Can this be scheduled/triggered automatically? | Add cron/scheduler |
| Can user input be pre-filled or inferred? | Implement smart defaults |
| Can repetitive tasks be batched? | Add bulk operations |
| Can manual data entry be replaced with imports? | Add CSV/API import |
| Can notifications replace polling/checking? | Implement push notifications |
| Can AI assist with decisions? | Add recommendation engine |
| Can workflows be templated? | Create workflow presets |
| Can integrations eliminate copy-paste? | Add API integrations |

### 3. Continuous Improvement Loop

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CONTINUOUS IMPROVEMENT CYCLE                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│    ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐  │
│    │ RESEARCH │ ──► │  BUILD   │ ──► │ ANALYZE  │ ──► │ IMPROVE  │  │
│    └──────────┘     └──────────┘     └──────────┘     └──────────┘  │
│          ▲                                                   │       │
│          └───────────────────────────────────────────────────┘       │
│                                                                      │
│  Research: Study domain, competitors, best practices                 │
│  Build: Implement with automation-first approach                     │
│  Analyze: Gather metrics, user feedback, performance data            │
│  Improve: Refine based on findings, then research again              │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Research Methodologies

### Competitive Analysis Template

```markdown
## Competitive Analysis: [App Name]

### Direct Competitors
| Competitor | Strengths | Weaknesses | Key Features We Should Consider |
|------------|-----------|------------|--------------------------------|
| [Name] | | | |

### Indirect Competitors / Adjacent Solutions
| Solution | How Users Currently Solve This | Opportunity |
|----------|-------------------------------|-------------|
| | | |

### Open Source Alternatives
| Project | Stars/Activity | What We Can Learn |
|---------|---------------|-------------------|
| | | |

### Academic Research / Papers
| Paper/Resource | Key Insight | Application to Our App |
|----------------|-------------|----------------------|
| | | |

### Technology Trends
- Emerging technologies relevant to this domain
- APIs/services that didn't exist 2 years ago
- Patterns from successful apps in adjacent spaces
```

### Domain Research Template

```markdown
## Domain Research: [Feature/App Area]

### Problem Definition
- Who has this problem?
- How severe is it? (frequency, impact)
- What triggers the need?
- What does success look like?

### Current Solutions (Manual Workflow)
1. Step user currently takes
2. Step user currently takes
3. → **AUTOMATION OPPORTUNITY**

### Best Practices Discovered
- Industry standards
- Regulatory requirements
- Accessibility guidelines
- Security considerations

### Data Sources Available
- Public APIs
- Datasets
- Services
- Standards/Schemas

### Recommended Approach
Based on research, the optimal solution is...
```

---

## Automation Patterns

### 1. Smart Defaults & Pre-filling

```python
# BEFORE: User fills everything manually
def create_invoice():
    # User must enter: date, number, terms, company info...
    pass

# AFTER: Intelligent automation
def create_invoice():
    return {
        "date": datetime.now(),
        "number": generate_next_invoice_number(),
        "due_date": datetime.now() + timedelta(days=get_user_default_terms()),
        "from": get_saved_company_info(),
        "tax_rate": get_regional_tax_rate(user.location),
        "currency": detect_currency_from_client(client),
        "template": get_most_used_template(),
    }
```

### 2. Batch Operations

```python
# BEFORE: One at a time
def send_email(recipient): ...

# AFTER: Batch with progress
def send_emails(recipients, template, options):
    batch = EmailBatch(recipients)
    batch.personalize(template)
    batch.schedule(options.send_time or optimal_send_time())
    batch.set_retry_policy(exponential_backoff)
    return batch.execute_async()
```

### 3. Workflow Automation

```yaml
# Define common workflows as templates
workflows:
  new_customer_onboarding:
    trigger: customer.created
    steps:
      - send_welcome_email
      - create_default_project
      - schedule_onboarding_call
      - add_to_newsletter
      - notify_sales_team
    
  invoice_overdue:
    trigger: invoice.overdue_7_days
    steps:
      - send_reminder_email
      - notify_account_manager
      - schedule_followup_task
```

### 4. Integration Automation

```python
# Eliminate copy-paste between systems
integrations = {
    "accounting": {
        "export": ["quickbooks", "xero", "freshbooks"],
        "sync": "bidirectional",
        "frequency": "real-time"
    },
    "crm": {
        "export": ["salesforce", "hubspot", "pipedrive"],
        "data": ["contacts", "deals", "activities"]
    },
    "calendar": {
        "sync": ["google", "outlook", "apple"],
        "features": ["scheduling_links", "availability"]
    }
}
```

### 5. AI-Assisted Automation

```python
# Use AI to reduce manual decisions
class SmartCategorizer:
    """Auto-categorize transactions, emails, tasks, etc."""
    
    def categorize(self, item):
        # Learn from user's past categorizations
        similar = self.find_similar_items(item)
        if similar.confidence > 0.9:
            return similar.category  # Auto-apply
        else:
            return self.suggest_with_confirmation(similar.category)

class SmartScheduler:
    """Find optimal times automatically"""
    
    def schedule(self, meeting):
        participants = meeting.participants
        preferences = self.get_preferences(participants)
        availability = self.get_availability(participants)
        return self.optimize(preferences, availability, meeting.constraints)
```

---

## Improvement Recommendations Framework

### After Every Feature, Generate:

```markdown
## Improvement Recommendations: [Feature Name]

### Immediate Improvements (Low effort, High impact)
1. [ ] Improvement with effort estimate
2. [ ] Improvement with effort estimate

### Medium-Term Improvements (Moderate effort)
1. [ ] Enhancement that builds on current feature
2. [ ] Integration opportunity

### Strategic Improvements (High effort, Transformative)
1. [ ] Major enhancement based on research
2. [ ] Competitive advantage opportunity

### Automation Opportunities Identified
| Manual Process | Proposed Automation | Effort | Impact |
|---------------|---------------------|--------|--------|
| | | | |

### Research-Driven Suggestions
Based on analysis of [competitors/research], consider:
- Feature X that 3/5 competitors have
- Best practice Y from industry standard
- Emerging technology Z that enables new capabilities
```

---

## Integration with Development Workflow

### Pre-Sprint/Pre-Feature

```bash
# Agent automatically runs research phase
1. Web search for "[feature domain] best practices 2024"
2. GitHub search for popular open-source implementations
3. Product Hunt / G2 search for commercial solutions
4. Check for relevant APIs (RapidAPI, public-apis list)
5. Review accessibility guidelines (WCAG)
6. Check regulatory requirements if applicable
```

### During Development

```bash
# Continuous automation checks
- Before manual input field → Can this be auto-filled?
- Before repetitive code → Can this be abstracted/automated?
- Before user workflow → Can steps be eliminated/combined?
- Before external dependency → Is there a better alternative?
```

### Post-Feature

```bash
# Improvement generation
1. Document what was built
2. List manual processes that remain
3. Research what competitors added recently
4. Generate improvement backlog
5. Prioritize by effort/impact
```

---

## Research Sources & Tools

### For Web Research
- **General**: Search engines, Wikipedia, industry publications
- **Technical**: Stack Overflow, GitHub, dev.to, HackerNews
- **Products**: Product Hunt, G2, Capterra, AlternativeTo
- **Academic**: Google Scholar, arXiv, ACM Digital Library
- **Standards**: W3C, IETF RFCs, ISO standards
- **APIs**: RapidAPI, public-apis repository, ProgrammableWeb

### For Automation Ideas
- **Zapier/Make templates**: Common integration patterns
- **IFTTT recipes**: Consumer automation patterns  
- **n8n workflows**: Open-source automation examples
- **GitHub Actions**: CI/CD automation patterns

### For Competitive Intelligence
- **BuiltWith**: Technology stack analysis
- **SimilarWeb**: Traffic and engagement data
- **Crunchbase**: Funding and company info
- **App stores**: Reviews and feature lists

---

## Agent Prompts for Research Mode

### Initial Research Prompt
```
Before implementing [FEATURE], conduct comprehensive research:

1. DOMAIN ANALYSIS
   - What problem does this solve?
   - Who are the users and what are their workflows?
   - What are industry best practices?

2. COMPETITIVE LANDSCAPE
   - What existing solutions exist?
   - What do they do well? Poorly?
   - What features are table-stakes vs. differentiators?

3. TECHNOLOGY ASSESSMENT  
   - What's the best technical approach?
   - What APIs/services can we leverage?
   - What open-source solutions exist?

4. AUTOMATION OPPORTUNITIES
   - What manual steps can be eliminated?
   - What can be scheduled/triggered automatically?
   - What integrations would save users time?

5. IMPROVEMENT ROADMAP
   - What's the MVP?
   - What are logical next improvements?
   - What would make this best-in-class?

Output a research report before any implementation.
```

### Automation Audit Prompt
```
Review [CODEBASE/FEATURE] for automation opportunities:

For each user-facing workflow:
1. List all manual steps required
2. Identify which can be automated
3. Propose specific automation implementation
4. Estimate effort and impact

For each data entry point:
1. Can it be pre-filled from existing data?
2. Can it be imported from external sources?
3. Can AI assist with suggestions?

For each repetitive operation:
1. Can it be batched?
2. Can it be scheduled?
3. Can it be triggered automatically?

Output actionable automation recommendations with priority.
```

### Improvement Generation Prompt
```
Generate improvement recommendations for [APP/FEATURE]:

Based on:
- Current implementation analysis
- Competitive research
- User workflow analysis
- Technology trends

Produce:
1. Quick wins (< 1 day effort)
2. Medium improvements (1-5 days)
3. Strategic enhancements (> 5 days)
4. Research-driven innovations

For each recommendation include:
- Description
- Rationale (why this matters)
- Effort estimate
- Expected impact
- Dependencies
```

---

## Quality Checklist

Before considering any feature complete:

```markdown
## Research & Automation Checklist

### Research Completed
- [ ] Domain/problem thoroughly understood
- [ ] Competitors analyzed
- [ ] Best practices identified
- [ ] Available APIs/services evaluated
- [ ] Security/accessibility considered

### Automation Maximized
- [ ] All possible smart defaults implemented
- [ ] Batch operations available where applicable
- [ ] Scheduling/triggers for repetitive tasks
- [ ] Import/export for data entry reduction
- [ ] Integrations for cross-system automation

### Improvements Documented
- [ ] Immediate improvements backlog created
- [ ] Medium-term roadmap documented
- [ ] Strategic opportunities identified
- [ ] Technical debt tracked

### User Value Validated
- [ ] Manual steps minimized
- [ ] Time-to-value optimized
- [ ] Learning curve reduced
- [ ] Power user paths available
```

---

## Example: Applying to Invoice App

### Research Output
```markdown
## Research: Invoice Management App

### Competitive Analysis
- FreshBooks: Great UX, expensive, limited automation
- Wave: Free, basic features, manual entry heavy
- Zoho Invoice: Feature-rich, complex UI
- **Opportunity**: None have AI-powered automation

### Automation Opportunities Found
1. Auto-fill from past invoices (client, items, terms)
2. Recurring invoice scheduling
3. Payment reminders (auto-escalating)
4. Expense receipt scanning + auto-categorization
5. Bank feed integration for reconciliation
6. Client payment portal (eliminate follow-ups)
7. Tax calculation by jurisdiction
8. Multi-currency with auto-conversion

### Recommended MVP Automations
1. Smart defaults from client history
2. Recurring invoices
3. Automated payment reminders
4. Payment link in invoice

### Future Improvements
1. AI categorization of line items
2. Predictive cash flow
3. Accounting software sync
4. Receipt OCR and matching
```

---

## Summary

**This skill ensures agents:**

1. ✅ **Research thoroughly** before building anything significant
2. ✅ **Automate aggressively** - treat manual processes as bugs
3. ✅ **Learn from ecosystem** - competitors, open-source, research
4. ✅ **Improve continuously** - generate improvement backlogs
5. ✅ **Document findings** - research reports, recommendations

**The goal: Build applications that are smarter than the user expects, with automation that anticipates their needs.**

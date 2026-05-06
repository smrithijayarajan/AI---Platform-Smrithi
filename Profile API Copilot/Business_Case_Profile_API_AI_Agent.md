# BUSINESS CASE: Profile API AI Agent

**Phase 1:** Internal Technical Consultants | **Phase 2:** Customers & Partners

**Author:** @Jayarajan, Smrithi  
**Lead Architect:** @Laudya, Ravi

---

## Executive Summary

This business case proposes the development and deployment of a **Profile API AI Agent**— an intelligent, context-aware assistant designed to guide technical consultants, customers, and partners through complex API usage, field requirements, and error resolution.

**Long Term Vision:** While initially focused on Profile APIs, this idea can be extensible across other APIs within SAP Concur, to support additional APIs across the Concur ecosystem. This approach unlocks multiplier value across the organization, reducing integration friction, lowering support costs, and accelerating partner and customer adoption of the broader API ecosystem.

The driver for this investment is clear: recurring misuse of the Profile API, confusion over field requirements, and heavy reliance on consultant intervention is consuming significant support resources. The projected outcome is **$60,000 per month in cost savings for the support team** — equivalent to **$720,000 annually**.

| Metric | Value |
|--------|-------|
| **Monthly Cost Savings (Support Team)** | $60,000 |
| **Annual Cost Savings** | $720,000 |
| **Primary Beneficiaries - Phase 1** | Internal Technical Consultants |
| **Primary Beneficiaries - Phase 2** | Customers & Partners |
| **Investment Horizon** | 2 Phases |

---

## Problem Statement

### 1.1 API Usage Misunderstanding

Both internal consultants and external clients frequently misunderstand the correct API usage patterns. A recurring example is the confusion between **user provisioning** and **identity creation and patching** — two distinct operations that are often used interchangeably, resulting in integration failures and downstream errors that require extensive hands-on guidance to resolve.

### 1.2 Difficulty Selecting the Right API

The API ecosystem offers numerous endpoints — including identity, orchestrated get, spend user, and travel profile — each designed for specific use cases. Customers and partners routinely struggle to identify the correct API for their scenario. This challenge is compounded by difficulty in locating relevant documentation, leading to a high volume of support requests and reactive consultant involvement.

### 1.3 Required Field Confusion

A significant source of integration failures stems from discrepancies between the fields marked as required in the API specification versus those required in the user interface form. Users encountering these inconsistencies frequently fail their API calls without understanding why, necessitating walkthrough sessions of configuration reports and form validation with consultants.

### 1.4 Time Consuming Documentation

Despite a library of technical documentation, customers and partners are currently overwhelmed by the volume and fragmentation of Profile API resources (Identity, Spend, Travel, etc.). The time required to manually locate, cross-reference, and validate specific API requirements has created a 'support bottleneck.

| Pain Point | Impact | Current Resolution |
|------------|--------|-------------------|
| API usage confusion | Integration failures, rework | Consultant walkthrough required |
| Wrong API selection (identity, orchestrated get, spend user, travel profile) | Wasted effort, support tickets | Consultant guidance + docs search |
| Required field mismatches (API vs. UI form) | Frequent API call failures | Configuration report review |
| No self-service error resolution | Escalation to support team | Direct consultant intervention |

---

## Proposed Solution

The **Profile API AI Assistant** is an intelligent, conversational assistant. It provides real-time, contextual guidance for API selection, field requirements, and error resolution — reducing reliance on human intervention for common scenarios.

### Core Capabilities

- **API guidance:** Explains when and how to use each API endpoint (identity, orchestrated get, spend user, travel profile, user provisioning)
- **Field requirement mapping:** Surfaces the correct required fields for any given API call, reconciling discrepancies between API specs and UI forms
- **Error diagnosis:** Identifies common error patterns and suggests step-by-step corrective actions grounded in documentation and knowledge base articles
- **Self-service resolution:** Enables users to resolve issues independently without escalating to a consultant
- **Documentation surfacing:** Proactively links to relevant documentation pages, reducing search time

### Phased Delivery Approach

|  | **Phase 1** | **Phase 2** |
|--|-------------|-------------|
| **Audience** | Internal Technical Consultants | Customers & Partners |
| **Primary Goal** | Reduce consultant lookup time and escalations | Reduce inbound support tickets & improve self-service |
| **Access Model** | Internal tooling / consultant portal | Customer-facing portal / partner developer hub |
| **Training Data** | Internal knowledge base, configuration docs | Phase 1 learnings + customer-facing documentation |
| **Success Metric** | Reduction in internal escalations | $60,000/month support cost savings |

---

## Financial Case

### Projected Cost Savings

The primary financial benefit of this initiative is a reduction in support team costs driven by decreased consultant intervention for recurring, solvable API-related issues.

| **Monthly Support Cost Savings** | **$60,000 / month** |
|----------------------------------|---------------------|
| Reduction in consultant hours and escalations related to API guidance and error resolution |

| **Annualized Cost Savings** | **$720,000 / year** |
|------------------------------|---------------------|
| Equivalent full-year value at projected monthly savings rate |

### Savings Drivers

- Reduction in repetitive consultant-led walkthroughs for API selection and field validation
- Decrease in inbound support tickets from customers and partners unable to locate documentation
- Faster error resolution — self-service deflection reduces case-handling time
- Reduced onboarding friction for new consultants and partner developers

### Cost-Benefit Summary

| Category | Monthly | Annual |
|----------|---------|--------|
| **Support Cost Savings** | $60,000 | $720,000 |
| **Estimated Maintenance & Hosting** | TBD | TBD |
| **Net Benefit (Conservative)** | $60,000 | $720,000 |

---

## Success Metrics

| Metric | Target | Phase |
|--------|--------|-------|
| Support ticket volume (API-related) | TBD | Phase 2 |
| Consultant escalation rate | ↓ 30% within 3 months | Phase 1 |
| Time-to-resolution for common API errors | ↓ 50% vs. baseline | Both |
| Monthly cost savings — support team | $60,000 | Phase 2 |

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Incomplete knowledge base coverage | Medium | High | Audit documentation gaps before Phase 1 launch; implement feedback loop |
| Low adoption by consultants (Phase 1) | Low | Medium | Early involvement of Vijay, Jonas and key consultants in design |
| PII information (Sensitive Information) query | Medium | Medium | Mask PII information in responses and flag high risk queries for appropriate response by assistant |

---

## Recommendation & Next Steps

We recommend proceeding with **Phase 1** of the Profile API AI Assistant. The problem is well-defined, the stakeholders are aligned, and the financial case is compelling. The phased approach de-risks delivery by validating the assistant with internal consultants before exposing it to customers and partners.

### Immediate Next Steps

- Define technical architecture and select AI tooling / platform
- Establish baseline metrics (ticket volume, escalation rate, resolution time) to measure impact
- Develop Phase 1 pilot plan targeting a defined consultant cohort for initial testing and feedback

**This initiative represents a high-confidence investment with a clear, measurable return.** With the right execution, it will materially reduce support costs, improve the experience for consultants and end users alike, and position the team for scalable, self-service API adoption.

# PRD: Enterprise CMS Platform Migration
**Author:** Thomas Edmons, Platform and Digital Product Manager  
**Status:** In Progress  
**Timeline:** 18 months | Target completion: June 2026  
**Last Updated:** February 2026

---

## Table of Contents
1. [Overview](#overview)
2. [Problem Statement](#problem-statement)
3. [Goals & OKRs](#goals--okrs)
4. [User Personas](#user-personas)
5. [Scope](#scope)
6. [Solution Overview](#solution-overview)
7. [Technical Architecture](#technical-architecture)
8. [Key Requirements](#key-requirements)
9. [Out of Scope](#out-of-scope)
10. [Success Metrics](#success-metrics)
11. [Risks & Mitigations](#risks--mitigations)
12. [Dependencies](#dependencies)
13. [Open Questions & Decisions](#open-questions--decisions)
14. [Appendix](#appendix)

---

## Overview

This document defines the product requirements for the end-to-end migration of an enterprise Content Management System (CMS) supporting a FinTech organization's public-facing website. The existing platform — a legacy monolithic CMS — is being replaced with a modern, composable architecture built on microservices, headless content management, and cloud-native infrastructure.

The migration encompasses 500+ webpages serving over 40 million annual visitors. It affects 12 content authors supporting 5 lines of business, and has direct implications for engineering velocity, operational cost, and customer experience performance.

---

## Problem Statement

The existing CMS platform has created compounding constraints across cost, performance, developer experience, and operational agility. At scale, these constraints have become critical blockers to the organization's ability to iterate quickly, maintain competitive web performance, and justify the platform's total cost of ownership.

**Core pain points:**

**Cost:** Annual enterprise license renewals represent over $1M in fixed platform costs with limited return on investment relative to modern open-source and SaaS alternatives.

**Developer experience:** The platform requires niche, specialized skillsets that are increasingly difficult to hire for and retain. This has created a single-point-of-failure risk in platform knowledge and slowed onboarding of new engineering contributors.

**Release velocity:** Development and deployment cycles are constrained to infrequent release windows — approximately once every two weeks — limiting the team's ability to respond to bugs, business needs, or performance regressions in a timely manner.

**Web performance:** Core Web Vitals scores on the platform's highest-trafficked pages are underperforming relative to industry benchmarks. Degraded Largest Contentful Paint (LCP) and Cumulative Layout Shift (CLS) scores have a measurable impact on the primary customer conversion action: login.

**Content authoring:** Content publishing workflows are labor-intensive, requiring disproportionate time per task. This reduces throughput for the content team and creates bottlenecks for business teams with time-sensitive publishing needs.

**Maintainability:** The platform's maintenance overhead — patching, upgrades, configuration management — consumes significant engineering capacity that could otherwise be directed toward product development.

---

## Goals & OKRs

### Objective 1: Reduce Platform Cost
**KR:** Eliminate $1.1M+ in annual enterprise license fees by fully decommissioning the legacy CMS upon migration completion.

### Objective 2: Optimize Operational Efficiency
**KR:** Reduce average content authoring task time by ≥50% for high-frequency use cases relative to the legacy platform baseline.  
**KR:** Reduce page-by-page design cycles by aligning to a systematic, component-driven design system approach.

### Objective 3: Improve Web Core Vitals
**KR:** Achieve a ≥40% improvement in page load speed on the highest-trafficked page.  
**KR:** Reduce LCP to within Google's "Good" threshold (≤2.5s) across top 20 pages by traffic volume.  
**KR:** Reduce CLS to within Google's "Good" threshold (≤0.1) across all migrated pages.

### Objective 4: Increase Speed to Market
**KR:** Increase available release windows from once every two weeks to a minimum of 3x per week.  
**KR:** Enable continuous deployment capability for content updates independent of code releases.

---

## User Personas

### Persona 1: Content Author
**Who they are:** A team of 12 content authors responsible for publishing and maintaining web content across 500+ pages and hundreds of targeted banner assets on behalf of 5 lines of business.  
**Goals:** Publish content quickly and accurately; work independently without needing engineering support for routine updates.  
**Pain points:** Legacy CMS workflows are slow, unintuitive, and require technical assistance for tasks that should be self-service. Learning curve on new tooling is real but time-to-proficiency is improving.  
**Success looks like:** Authoring tasks that previously took an hour now take under 30 minutes. Structured content models reduce ambiguity and publishing errors.

### Persona 2: Front-End / Platform Engineer
**Who they are:** Engineers responsible for building, maintaining, and deploying the web platform.  
**Goals:** Ship features quickly; work in modern, well-documented tooling; reduce time spent on platform maintenance.  
**Pain points:** Legacy platform required niche skillsets, had limited CI/CD integration, and constrained release frequency. Migration introduces new complexity in tooling and automation.  
**Success looks like:** Three deploys per week with confidence. Infrastructure managed as code via Terraform. AI-assisted development reducing implementation time on migration automation scripts.

### Persona 3: Line of Business Stakeholder
**Who they are:** Representatives from 5 internal business units (marketing, product, and user experience for each) who rely on the content team to execute web publishing.  
**Goals:** Get web content and campaigns live quickly; maintain brand consistency; have confidence in the platform's reliability.  
**Pain points:** Long release windows delayed time-sensitive campaigns. Inconsistent page designs created friction with brand application and operational efficiency.  
**Success looks like:** Requests are fulfilled faster. A systematic design approach reduces back-and-forth revision cycles.

### Persona 4: Site Visitor (End Customer)
**Who they are:** ~40M annual visitors to the unauthenticated public website, primarily prospective and existing customers of a FinTech product.  
**Goals:** Find information quickly; experience fast, reliable page loads; complete the login action without friction.  
**Pain points:** Slow page load times create drop-off before the two primary user actions: conversion-driving CTAs for prospects and login for existing customers.  
**Success looks like:** Pages load fast, render correctly across devices, and the path to both conversion CTAs and login is frictionless.

---

## Scope

### In Scope
- Full migration of 500+ existing webpages from the legacy CMS to the new platform stack
- Implementation of headless CMS (Contentful) as the content management layer
- Front-end rebuild using React, Next.js, and TypeScript
- Cloud infrastructure provisioning on AWS (CloudFront CDN, S3 storage)
- CI/CD pipeline integration with GitLab and Terraform
- Development of file storage interfaces and migration automation tooling
- Content author training and onboarding to Contentful
- Design system alignment across all migrated pages
- Business validation and UAT with 5 lines of business
- Performance benchmarking and Core Web Vitals validation pre- and post-migration

### Out of Scope
- Authenticated / logged-in product experience
- Backend application services beyond the web content layer
- Mobile native applications
- Third-party integrations not currently present on legacy platform

---

## Solution Overview

The new platform replaces the legacy monolithic CMS with a composable, microservices-oriented architecture. Each layer of the stack is purpose-built, independently deployable, and aligned to modern enterprise standards.

**Content layer (Contentful):** A headless CMS decouples content from presentation. Content authors work in a structured, intuitive interface. Content is delivered via API to the front end, enabling real-time publishing without a code deployment.

**UI layer (React / Next.js / TypeScript):** A component-driven front end built on Next.js enables server-side rendering (SSR) and static site generation (SSG) for optimized Core Web Vitals performance. TypeScript enforces type safety and improves developer experience and code maintainability.

**Infrastructure layer (AWS CloudFront + S3):** CloudFront provides global CDN distribution for low-latency content delivery. S3 serves as the file storage backbone for static assets. Infrastructure is defined and versioned as code using Terraform.

**CI/CD layer (GitLab + Terraform pipelines):** Standardized pipelines align the platform with enterprise DevOps practices, enabling frequent, reliable releases and reducing manual deployment overhead.

**Migration tooling:** Custom automation scripts and file storage interfaces are being built to facilitate the migration of 500+ pages at pace, with AI coding assistance accelerating development of migration utilities.

---

## Technical Architecture

```
                        ┌─────────────────────────────────┐
                        │         Content Authors          │
                        │         (Contentful CMS)         │
                        └────────────────┬────────────────┘
                                         │ Content API
                        ┌────────────────▼────────────────┐
                        │       Next.js / React / TS       │
                        │        (UI / Front End)          │
                        └────────────────┬────────────────┘
                                         │
               ┌─────────────────────────▼──────────────────────────┐
               │                  AWS CloudFront                     │
               │              (CDN / Edge Distribution)              │
               └──────────────┬──────────────────────┬──────────────┘
                               │                      │
               ┌───────────────▼──────┐   ┌──────────▼───────────────┐
               │       AWS S3         │   │     GitLab CI/CD          │
               │   (Static Assets /   │   │  + Terraform Pipelines    │
               │    File Storage)     │   │  (Build / Deploy / IaC)   │
               └──────────────────────┘   └──────────────────────────┘
```

---

## Key Requirements

### Functional Requirements

**Content Management**
- Content authors must be able to publish and update pages without engineering involvement for routine content changes
- Contentful content models must support all existing content types present across the 500+ page inventory
- Structured content components must map to the enterprise design system, replacing freeform page-by-page design
- Content preview capability must be available prior to publishing

**Front End**
- All migrated pages must render correctly across modern browsers and viewport sizes
- Next.js SSR/SSG must be configured appropriately per page type based on content update frequency and performance requirements
- TypeScript must be enforced across all new front-end code
- Component library must be aligned to and validated against the enterprise design system

**Infrastructure**
- CloudFront distribution must be configured for optimal caching behavior per content type
- S3 bucket policies and access controls must meet enterprise security requirements
- All infrastructure must be defined in Terraform and version-controlled in GitLab
- Zero-downtime deployment must be achievable for all production releases

**CI/CD**
- Pipeline must support a minimum of 3 production deployments per week
- Automated testing (unit, integration, accessibility) must be gated in the pipeline before production deployment
- Content-only updates via Contentful must not require a code deployment

### Non-Functional Requirements
- Page load performance: ≥40% improvement in load time on highest-trafficked page vs. legacy baseline
- LCP: ≤2.5s on top 20 pages by traffic
- CLS: ≤0.1 across all migrated pages
- Uptime SLA: ≥99.9% on production environment
- Migration completion: all 500+ pages migrated and validated by June 2026

---

## Out of Scope

- Any changes to authenticated user experience or logged-in product flows
- Redesign of existing content — migration preserves existing content with structural improvements only
- Legacy CMS decommission activities beyond the scope of the web platform (e.g. other internal tools using the legacy CMS)

---

## Success Metrics

| Metric | Baseline | Target | Status |
|---|---|---|---|
| Annual license cost | $1.1M+ | $0 (post-decommission) | 🟡 In progress |
| Release frequency | 1x / 2 weeks | 3x / week | ✅ Achieved |
| Page load speed (top page) | Baseline | ≥40% improvement | ✅ Achieved |
| Content authoring time | Baseline | ≥50% reduction (key use cases) | ✅ Achieved (key use cases) |
| LCP (top 20 pages) | Above threshold | ≤2.5s | 🟡 In progress |
| CLS (all pages) | Above threshold | ≤0.1 | 🟡 In progress |
| Pages migrated | 82 / 500+ | 500+ / 500+ | 🟡 In progress |

---

## Risks & Mitigations

**Risk: Organizational change resistance**  
*Impact: High | Likelihood: High (realized)*  
Content authors and business stakeholders showed low willingness to adopt new tooling and workflows at project onset. Conflicting opinions between UX and Engineering on design system adoption created early delays.  
*Mitigation:* Proactive stakeholder alignment sessions were conducted to mediate between UX and Engineering, resolving inconsistencies between the enterprise design system and prior page-by-page design patterns. Author training programs and change management communications were built into the project plan.

**Risk: Migration tooling complexity**  
*Impact: High | Likelihood: Medium (partially realized)*  
Migrating 500+ pages at pace requires custom automation tooling that did not exist at project start. Building file storage interfaces and migration scripts from scratch introduced scope and timeline risk.  
*Mitigation:* Engineering teams built custom automation scripts and file storage interfaces. AI coding assistance was introduced to accelerate development of migration utilities — a meaningful shift for an organization historically cautious in adopting new technologies.

**Risk: Design system inconsistency**  
*Impact: Medium | Likelihood: High (realized)*  
The legacy platform supported freeform, page-by-page design, creating significant inconsistency across the 500+ page inventory. Aligning to a systematic component-driven approach required reconciliation of existing designs.  
*Mitigation:* Close coordination between UX, Engineering, and PM to audit and reconcile design patterns. Component library locked to enterprise design system standards prior to migration of each page group.

**Risk: Zero-downtime migration cutover**  
*Impact: High | Likelihood: Low*  
Cutover of a 40M visitor/year website carries risk of downtime or degraded experience if not executed correctly.  
*Mitigation:* Phased migration approach with parallel running of legacy and new platform. Comprehensive testing gates (functional, performance, accessibility) prior to each page group cutover. Rollback procedures defined for each phase.

---

## Dependencies

| Dependency | Owner | Status |
|---|---|---|
| Contentful enterprise account provisioning | Platform Engineering | ✅ Complete |
| AWS infrastructure setup (CloudFront, S3) | Infrastructure / DevOps | ✅ Complete |
| GitLab CI/CD pipeline configuration | DevOps | ✅ Complete |
| Enterprise design system component library | UX / Design | ✅ Complete |
| Content inventory and audit (500+ pages) | Content Team + PM | ✅ Complete |
| Migration automation tooling | Engineering | 🟡 In progress |
| Business validation / UAT (5 LOBs) | Lines of Business | 🟡 In progress |
| Legacy CMS decommission plan | Platform Engineering + PM | 🔴 Not started |

---

## Open Questions & Decisions

| # | Question | Owner | Status |
|---|---|---|---|
| 1 | What is the page-by-page migration prioritization order across 500+ pages? | PM | ✅ Complete |
| 2 | What is the rollback strategy and criteria for triggering it during cutover? | Engineering | ✅ Complete |
| 3 | How will SEO equity (URLs, metadata, redirects) be preserved during migration? | PM + Engineering | 🟡 In progress |
| 4 | What is the sunset timeline for the legacy CMS post full migration? | PM + Finance | 🔴 Open |
| 5 | Will AI coding assist tooling be standardized post-project or remain experimental? | Engineering Leadership | 🔴 Open |

---

## Appendix

### Glossary
- **CMS:** Content Management System — software used to create, manage, and publish digital content
- **Headless CMS:** A CMS architecture where the content repository (back end) is decoupled from the presentation layer (front end), connected via API
- **SSR:** Server-Side Rendering — pages are rendered on the server at request time for improved performance and SEO
- **SSG:** Static Site Generation — pages are pre-rendered at build time for maximum performance
- **LCP:** Largest Contentful Paint — a Core Web Vitals metric measuring perceived load speed
- **CLS:** Cumulative Layout Shift — a Core Web Vitals metric measuring visual stability
- **IaC:** Infrastructure as Code — managing and provisioning infrastructure through machine-readable configuration files (Terraform)
- **CDN:** Content Delivery Network — a geographically distributed network of servers that delivers content to users based on proximity

### Related Documents
- [System Architecture Diagram](../system-design/)
- [Content Migration Runbook](../runbooks/content-migration)
- [Design System Alignment Audit](../design/system-audit)
- [Performance Benchmarking Report](../reports/core-web-vitals)

# Content Cluster Outline Q3 2025: Frontend Observability for Web

## Content

This self-paced, beginner-friendly cluster of activities teaches web developers and DevOps engineers how to instrument frontend applications for observability using Honeycomb’s Web JavaScript SDKs and OpenTelemetry.

Learners will build confidence in understanding browser telemetry, setting up client-side tracing, and connecting frontend spans to backend traces. Content is delivered via short videos (formal learning), hands-on labs (application), and blogs [maybe].

- **Format:** Multi-modal: Videos, labs, blogs [maybe]
- **Prerequisites:**
  - Familiarity with Browser-Based JavaScript: Understanding of how to write and run JavaScript in a browser environment.
  - Basic Web Application Structure: Awareness of how Single Page Applications (SPAs) or multi-page apps are structured (e.g., what’s in index.html, main.js, use of npm or bundlers like Vite/Webpack).
  - Understanding of HTTP Requests: Basic knowledge of how frontend apps make API requests using fetch or XMLHttpRequest.
  - Access to Modify and Deploy Frontend Code: Learners must have access to a codebase where they can install packages, edit source files, and observe trace data.
  - A Honeycomb account and API key.

## Needs Analysis

**Knowledge Gap:**
Frontend engineers are often familiar with performance monitoring but unfamiliar with distributed tracing and high-cardinality event telemetry.

**Evidence of Need:**
Many Honeycomb customers use backend tracing but haven’t adopted frontend tracing/o11y, despite performance blind spots starting at the browser.

## Problem & Solution

**Problem:**
Frontend developers and DevOps engineers lack clear, actionable guidance on getting started with observability in their web apps using Honeycomb.

**Solution:**
This cluster offers contextual, structured, and applied learning formats to drive HFO instrumentation skill adoption.

## Evaluation Plan

**Business Goal:** Expand Honeycomb adoption across full-stack environments

| KPI Category       | Metric                                                        |
|--------------------|---------------------------------------------------------------|
| Product Usage      | Number of customers instrumenting web apps using this content |
| Customer Sentiment | Post-course CSAT                                              |
| Support Deflection | Reduction in frontend instrumentation tickets                 |
| Impact             | Increase in instrumented frontend services                    |

## Learning Cluster: Topics & Assets

### Social/Contextual Learning: Motivation

- **Video: Why Frontend Observability**
  - Objectives:
    - Describe why frontend telemetry completes the observability picture — what problems will they be able to solve, and how can they solve them with HFO? [Answer: Fullstack tracing!]
    - Identify problems they will be able to solve using Honeycomb FO (e.g., browser latency, missing visibility).
  - Use Case: Frontend developer sees latency but has no browser-side visibility. 
    - Some work to be done: This use case needs to be flushed out with details that'll really help illustrate the problem for users.
    - Each use case/example we present: It should shape the problem for them (even if they didn't know it existed), ensure they understand the problem, *and* won't overwhelm them. The focus is on the solution, not the problem. 
  - Learning Gap: FE teams are even more distanced from observability than their BE counterparts.
  - Learning Gap: Make sure you understand the end-to-end value, the fullstack value is the real magic.
  - CTA: End with the end-the-end value.

### Formal Learning: Core Concepts

- **Video: How Frontend Tracing Works in Honeycomb**
  - Objectives:
    - Explain how frontend telemetry data becomes spans and traces
    - Describe the relationship between "raw" telemetry data and visualizations
    - Clarify difference between metrics dashboards and trace-based querying
    - Introduce trace, span, log, and metric data structure
  - Learning Gap: Helps take them from the RUM mindset to observability. To go from old ways to new, and understand their systems' behavior *better* through tracing.

- **Click-Through Tutorial: Architecture Diagram: Honeycomb Frontend Instrumentation Overview**
  - Objectives:
    - Identify each component in the Honeycomb Web SDK architecture (e.g., user events, OpenTelemetry Web SDK, exporter, collector)
    - Trace the path of a user interaction from the browser to Honeycomb ingest
    - Pinpoint where they can inject custom fields, manage sessions, or secure data
    - Understand where trace context originates and is propagated
  - Activity: Trace a user click through the architecture

- **Video: Automatic Instrumentation with Honeycomb Web SDKs + OpenTelemetry**
  - Objectives:
    - Describe types of telemetry automatically collected: traces, spans, metrics, logs. This is SUPER brief and should center FE data in these forms, but we do have lots of content on telemetry types, so we don't need to reinvent the wheel.
    - Explain how automatic instrumentation reduces setup effort
        - Automatic context propgation with backend spans* but they should double-check for headers!
        - Automatic session ID* but this should also be double-checked, and can be customized!
        - Uses OTel semantic conventions
    - Verify whether automatic instrumentation is working correctly: Can we see traces in Honeycomb?
    - Emphasize the need to double-check for (and/or add) trace propagation headers and baggage for session ID
  - Included in atuomatic instrumentation and should be mentioned:
    - Core Web Vitals instrumentation
    - Document load and navigation timing instrumentation
    - Click instrumentation
    - Context Enrichment: Browser and device attributes (e.g., user agent, page location, browser type)
    - Attributes:
        - HTTP methods, URLs, timings according to HTTP semconv
        - Resource timings
        - Errors
        - Browser attributes
        - Feature flags

- **Video: Can't See Your Full Trace? How Context Propagation Works** 
  - Objectives:
    - Diagnose missing trace segments or full traces
    - Validate correct context propagation between frontend and backend
    - Identify and correct common instrumentation pitfalls
  - Split into:
    1. Honeycomb SDK configuration and troubleshooting
    2. High-level OpenTelemetry context propagation strategy
  - Use Case: I'm an FE engineering who added automatic instrumentation, but... I! can't! see! my! full! trace! and I don't understand why. What can I look into to clarify my understanding of instrumentation?
  - Include: Passing context around properly throughout the frontend... Means you need to propagate trace IDs correctly.
  - This is a trade header, this is how propagation works, this is context between services.
  - Must use good, real-world examples!
    - Empahsize their mental model: Think of it as a Swiss-Army knife. If you want it to look like this, here's an example of a thing that DOES do that.  

- **Video: Defining a Session for Your App** 
  - Objectives:
    - Override Honeycomb’s default session ID generator
    - Persist session IDs using browser storage
    - Align session attribution with their app’s user or visitor session model
    - Confirm shared session context across interactions
  - Note: Address logic for authenticated vs anonymous users
  - Note: Really, the goal here is: Give folks tool to hone their telemetry.

```javascript
import { WebSDK } from '@honeycombio/web-sdk';

new WebSDK({
  sessionIdGenerator: () => {
    return window.localStorage.getItem('session_id') || uuidv4();
  },
  // other config...
});
```

- **Video: Adding Custom Attributes to Spans**
  - Objectives:
    - Show three examples of customization: basic user info, feature flags, and app-specific context
    - Demonstrate how to safely add attributes using OpenTelemetry
    - Emphasize naming consistency and privacy considerations
    - Explain why custom attributes matter for observability - this might be hard to explain in the abstract.
- **Example:** `user.id`, `feature.flag`, `checkout.step`

- **Blog/Doc: Best Practices for Frontend Tracing**
  - Objective: Apply naming conventions and field clarity
    - **Best Practices:**
      - Use semantic names
      - Initialize early
      - Propagate trace context
      - Configure baggage
      - Use semantic conventions

### Application Learning: Hands-on Labs

- **Lab: Setup Honeycomb Browser Web SDK [note: we need a FE app - Meminator, pending DevRel support to stand it up]**
  - Objective:** Instrument app with Honeycomb Web SDK & OTel
    - **Best Practices:**
      - Initialize early
      - Use a stable `service.name`
      - Propagate `trace.id`
      - Use `session_id`
      - Use Collector for proxying
      - Highlight benefits of using a Collector
  - **Knowledge Check:** Spans link to backend
  - **Knowledge Check:** Traces visible in Honeycomb

- **Lab: Adding Custom Attributes to Spans**
  - Objective: Add user/feature-specific metadata
  - Levels of depth:
    1. Basic user ID
    2. Feature flags or A/B groups
    3. Custom business logic or app state

```javascript
import { trace } from '@opentelemetry/api';
const span = trace.getSpan(trace.context.active());
if (span) {
  span.setAttribute('user.id', userId);
  span.setAttribute('feature.flag', 'new-dashboard');
}
```

## Subject Matter Experts

- Eng: Wolfgang
- EM: Katie
- PM: Taylor (Interim)
- DevRel: Ken
- Docs: Eric


## Resources
<image src=".github/assets/HFO instrumentation setup.png">

https://www.honeycomb.io/blog/manage-your-session-id-honeycombs-web-sdk
http://docs.google.com/presentation/d/19wC7RbvY6hHguLpcvNx2ofnKubzAUUC-ZGsQdZdMCXA/edit#slide=id.g331e3030cc4_1_307
https://www.figma.com/slides/fqURh1CXdoxkJGTjaIrK3x/Instrumentation-in-poodle-js-presentation?node-id=1-1299&t=zZlrMfEGjVQAPLwA-0 

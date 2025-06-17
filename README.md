# Content Cluster Outline Q3 2025: Frontend Observability in Honeycomb

## Content  
This self-paced, beginner-friendly cluster of activities teaches web developers and DevOps engineers how to instrument frontend applications for observability using Honeycomb’s JavaScript SDKs and OpenTelemetry. Learners will build confidence in understanding event-based telemetry, setting up client-side tracing, and connecting frontend spans to backend traces. Content is delivered via short videos (formal learning), hands-on labs (application), and reference PDFs.

- **Format:** Multi-modal: Videos, labs, reference PDFs (?)
- **Prerequisites:** 
    - Familiarity with Browser-Based JavaScript: Understanding of how to write and run JavaScript in a browser environment (e.g., using console.log, fetch, event handlers).
    Basic Web Application Structure: Awareness of how Single Page Applications (SPAs) or multi-page apps are structured (e.g., what’s in index.html, main.js, use of npm or bundlers like Vite/Webpack).
    Understanding of HTTP Requests: Basic knowledge of how frontend apps make API requests using fetch or XMLHttpRequest.
    Access to Modify and Deploy Frontend Code: Learners must have access to a codebase where they can install packages, edit source files, and observe trace data.
    A Honeycomb account and API key.

## Needs Analysis  
**Knowledge Gap:**  
Frontend engineers are often familiar with performance monitoring but unfamiliar with distributed tracing and high-cardinality event telemetry.  

**Evidence of Need:**  
Many Honeycomb customers use backend tracing but haven’t adopted frontend telemetry, despite performance blind spots starting at the browser.

## Problem & Solution  
**Problem:**  
Frontend developers and DevOps engineers lack clear, actionable guidance on getting started with observability in their web apps using Honeycomb.

**Solution:**  
This cluster offers contextual, structured, and applied learning formats to drive skill adoption.

## Evaluation Plan

**Business Goal:** Expand Honeycomb adoption across full-stack environments

| KPI Category       | Metric                                                           |
|--------------------|------------------------------------------------------------------|
| Product Usage      | Number of customers instrumenting web apps using this content |
| Customer Sentiment | Post-course CSAT                                              |
| Support Deflection | Reduction in frontend instrumentation tickets               |
| Impact             | Increase in instrumented frontend services                   |

## Learning Cluster: Topics & Assets

### Social/Contextual Learning: Motivation

- **Video: Why Frontend Observability Matters**  
  Objective: Describe why frontend telemetry completes the observability picture.  
  Use Case: Dev sees latency but has no browser-side visibility.
  Learning Gap: FE teams are even more distanced from observability than their BE counterparts. There is some responsibility here to convice learners that o11y for the frontend (really, using traces) is the way to go. 

### Formal Learning: Core Concepts

- **Video: What Honeycomb’s Web SDK Automatically Instruments**  
  Objectives:
    - Describe types of telemetry automatically collected
    - Explain how automatic instrumentation reduces setup effort
    - Verify whether automatic instrumentation is working correctly
  Include:
    - Performance Metrics: Core Web Vitals (LCP, FID, CLS)
    -     - Lifecycle Events: Document load and navigation timing
    - Network Requests: HTTP fetch and XMLHttpRequest calls
    - Error Capture: Global JS errors and uncaught exceptions
    - Context Enrichment: Browser and device attributes (e.g., user agent, screen size)
    - Session Attribution: Automatic session ID assignment with customization options
  Note: Clarify what users get “for free.”
  Learning Gap: Many frontend engineers assume that instrumentation means manually defining every span, field, or metric. They may not realize how much Honeycomb’s Web SDK gives them out-of-the-box, or how to confirm it’s working. This leads to: Redundant or unnecessary custom spans, uncertainty around what data is already being sent, frustration when “expected” events (like page loads or errors) don’t show up because the SDK wasn’t initialized correctly

- **Click-Through Tutortial: Architecture Diagram: Honeycomb Frontend Instrumentation Overview**  
  - Objectives:
    - Identify each component in the Honeycomb Web SDK architecture (e.g., user events, OpenTelemetry Web SDK, exporter, collector)
    - Trace the path of a user interaction from the browser to Honeycomb ingest
    - Pinpoint where they can inject custom fields, manage sessions, or secure data
    - Understand where trace context originates and is propagated
  - Activity: Trace a user click through the architecture

- **Video: How Frontend Tracing Works in Honeycomb**  
  - Objectives: 
    - Explain how frontend telemetry data becomes spans and traces  
    - Describe the relationship between raw telemetry data and visualizations
    - Identify what kinds of data they need to send to reflect business or technical problems
    - Use Honeycomb’s event model to shape a trace-based visualization
  Learning Gap: Frontend engineers often expect telemetry to be plug-and-play — that once instrumentation is added, Honeycomb will “just show” the right charts and answers. But Honeycomb (via OpenTelemetry) exposes raw, high-cardinality events — not opinionated dashboards. This delta in expectations causes confusion, disorientation, and often discouragement.
    - Learner’s mental model: “Telemetry is pre-shaped dashboards, like metrics tools.”
    - Actual model: “Telemetry is flexible event data that you must shape with queries.”
    - Frustration pattern: “I added tracing. Why don’t I see page load times? Where’s the funnel chart? Why isn’t the thing I care about here by default?”
  
- **Video: Understanding Dependencies Between Frontend Services**  
  Objectives:
  - Reveal frontend component interactions by way of instrumentation and viewing traces in Honeycomb
  - Describe how frontend spans can expose relationships between frontend components, services, or libraries 
  - Use trace views in Honeycomb to identify interactions and timing between frontend elements
  - Apply this understanding to performance tuning or debugging coordination issues
  Use Case: SPA/microfrontend coordination  
  Learning Gap: Frontend teams often build components or microfrontends in isolation — even when they run in the same browser. When performance issues or bugs occur, engineers may lack visibility into how different pieces interact or affect one another. Tracing can help them spot latency introduced by one team’s module affecting another’s, debug coordination issues (e.g., race conditions, unhandled async flows), measure perceived user experience across multiple async frontend systems
  Note: Show trace examples

  **Video: Why Can’t I See My Full Trace? Fixing Instrumentation Gaps**
  Objectives:
    - Diagnose why a trace might not appear end-to-end in Honeycomb
    - Validate correct context propagation between frontend and backend
    - Identify and correct common instrumentation pitfalls
  Learning Gap: Learners often expect traces to “just connect” once they’ve instrumented frontend and backend services. But in practice, they often encounter broken trace chains, leading to missing segments, or completely disconnected frontend and backend traces.
    - Learner frustration: “Why don’t I see the whole trace?”
    - Actual root causes:
    1. Missing or incorrect context propagation (e.g., headers not sent)
    2. Backend not configured to extract trace context
    3. Timing mismatch in span duration/nesting
    4. SDK initialized too late in the app lifecycle

**Video: Adding HFO Custom Session IDs**
Objectives:
  - Override Honeycomb’s default session ID generator
  - Persist session IDs using browser storage
  - Align session attribution with their app’s concept of a user or visitor session
  - Confirm that spans and traces share session context across interactions
Learning Gap: Honeycomb automatically generates session IDs in the Web SDK, but some customers need more control (e.g., to align with app auth sessions or marketing campaigns); others don’t realize how powerful consistent session attribution can be for tracking user flows; and many fail to persist session IDs correctly across reloads or tabs, which fragments traces. W/o deliberate session ID management, it's hard to reconstruct full user journeys, analyze retention or conversion, and debug flakiness tied to multi-page flows.
Example code snippet:
```javascript
import { WebSDK } from '@honeycombio/web-sdk';

new WebSDK({
  sessionIdGenerator: () => {
    return window.localStorage.getItem('session_id') || uuidv4();
  },
  // other config...
});
```

- **Blog: Best Practices for Frontend Tracing**  
  Objective: Apply naming conventions and field clarity  
  Best Practices: Use semantic names; initialize early

### Application Learning: Hands-on Labs

Note: These require a sample application written in Browser JS. We need to find engineering resources to write this application, then test instrument it.

- **Lab: Setup for Honeycomb JS SDK**  
  Objective: Instrument app with Honeycomb JS SDK  
  Best Practice: Initialize early. General flow: Send your data in traces with a single `service.name`, initiate your SDK, add a `session_id` that rotates, include important web metrics, propagate your `trace.id` and make it connect with your backend `trace.ids`.
  Knowledge Check: Traces visible in Honeycomb

- **Lab: Setup for OpenTelemetry Web SDK**  
  Objective: Configure OTel Web SDK and export to Honeycomb  
  Best Practice: Use Collector for proxying  
  Knowledge Check: Spans link to backend

- **Lab: Adding Custom Attributes to Spans**  
  Objective: Add user/feature-specific metadata  
  Example:
  ```javascript
  import { trace } from '@opentelemetry/api';
  const span = trace.getSpan(trace.context.active());
  if (span) {
    span.setAttribute('user.id', userId);
    span.setAttribute('feature.flag', 'new-dashboard');
  }

## Resources
{{<image src=".github/assets/HFO instrumentation setup.png">}}

https://www.honeycomb.io/blog/manage-your-session-id-honeycombs-web-sdk
http://docs.google.com/presentation/d/19wC7RbvY6hHguLpcvNx2ofnKubzAUUC-ZGsQdZdMCXA/edit#slide=id.g331e3030cc4_1_307
# Video: How Frontend Tracing Works in Honeycomb

## Learning Objectives
- Explain how frontend telemetry data becomes spans and traces
- Describe the relationship between raw telemetry data and visualizations
- Clarify difference between metrics dashboards and trace-based querying
- Introduce trace, span, log, and metric data structure

---

**[Visual]** Title screen: "How Frontend Tracing Works in Honeycomb"  
**[Audio]** "Let’s break down how frontend tracing actually works - and why it’s different from traditional performance monitoring."

**[Visual]** Browser screenshot with standard metrics dashboard: Core Web Vitals chart, Lighthouse score, frame rate.  
**[Audio]** "Most frontend tools focus on Real User Monitoring, or RUM — surface-level metrics like LCP, TTI, and CLS."

**[Visual]** Parallel panels: one shows a bar chart of LCP values, the other a Honeycomb trace timeline.  
**[Audio]** "Metrics show you *what* happened — like slow page loads. But they don’t show you *how* or *why*."

**[Visual]** Side-by-side dashboards: Left shows a high-level metric chart; Right shows Honeycomb query interface with trace breakdowns.  
**[Audio]** "Metrics dashboards are great for trend analysis — you can spot anomalies over time. But they’re rigid: the data is pre-aggregated, so you can’t dig into individual user experiences."

**[Visual]** Honeycomb Query Builder filters down to one trace. Spans are clickable. Trace graph expands.  
**[Audio]** "Trace-based querying, on the other hand, is flexible and exploratory. You can slice, filter, and pivot by any attribute — like user ID, session, or browser type — and see the exact flow of a request."

**[Visual]** Text overlay with telemetry types: Traces, Metrics, Logs, Events. Icons illustrate each.  
**[Audio]** "Let’s talk telemetry. Each type gives you a different lens into your application."

**[Visual]** Metric: Graph showing average LCP over time.  
**[Audio]** "Metrics are great for tracking trends — they aggregate data like load times or error rates across users and sessions."

**[Visual]** Log: DevTools console view, then a structured JSON log message.  
**[Audio]** "Logs are records of what happened. They're detailed, timestamped, and often unstructured."

**[Visual]** Trace: Honeycomb UI showing a full trace with frontend and backend spans.  
**[Audio]** "Traces are different. They follow a single request or user interaction from end to end — across services, spans, and systems."

**[Visual]** Span anatomy breakdown: span ID, name, start time, duration, attributes like user.id and page URL.  
**[Audio]** "Each span is a structured event — it captures start time, duration, and key attributes for one unit of work — like a click, render, or fetch."

**[Visual]** Browser dev events labeled: click, DOMContentLoaded, fetch. Mapping arrows to spans.  
**[Audio]** "When you use the Honeycomb Web SDK, those raw browser events are automatically transformed into spans using OpenTelemetry."

**[Visual]** Honeycomb trace view: click → documentLoad → fetch → backend.  
**[Audio]** "Those spans are linked together by context — and become a trace that tells a full story, from the user’s interaction to the server’s response."

**[Visual]** JSON payload of telemetry: `name`, `startTime`, `duration`, `attributes`. Transition to span in Honeycomb.  
**[Audio]** "Behind every span is raw telemetry data — JSON payloads with timestamps, field names, and context. Honeycomb turns that into a visual timeline you can search, filter, and explore."

**[Visual]** Side-by-side: left shows a metric graph spike; right shows the corresponding trace with clear bottleneck.  
**[Audio]** "A metric can tell you response time spiked. A trace can show you *which part* of the experience caused the spike — and why."

**[Visual]** Span detail panel showing structured telemetry attributes.  
**[Audio]** "That’s the power of structured telemetry — when you attach user and session context to spans, they become searchable and actionable."

**[Visual]** Trace includes logs inside a span and a metric attached as an attribute.  
**[Audio]** "And in Honeycomb, spans can include logs and metrics, too — so you can correlate across all three types."

**[Visual]** Diagram: browser → OpenTelemetry Web SDK → Honeycomb exporter → trace view.  
**[Audio]** "The Honeycomb Web SDK uses OpenTelemetry to collect and send telemetry from the browser — turning raw events into spans."

**[Visual]** Highlight trace context: `trace_id`, `span_id`, `service.name`, user-defined fields.  
**[Audio]** "Each event carries trace context — so frontend and backend spans stitch together into a single trace. That’s fullstack tracing."

**[Visual]** Jess reviews a complete trace in Honeycomb, with spans labeled and contextual attributes visible.  
**[Audio]** "For frontend engineers, that means you can see what happened from the moment a user clicked — all the way to the backend response."

**[Visual]** Final screen: "Next: Architecture Overview"  
**[Audio]** "Now that you know how tracing works — and how it compares to other telemetry — let’s look at how the frontend stack fits together."
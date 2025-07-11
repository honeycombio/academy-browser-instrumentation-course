# Video: How Frontend Tracing Works in Honeycomb

## Learning Objectives
- Explain how frontend telemetry data becomes spans and traces
- Describe the relationship between raw telemetry data and visualizations
- Clarify difference between metrics dashboards and trace-based querying
- Introduce trace, span, log, and metric data structure

---

**[Visual]** Split screen: frustrated developer looking at Core Web Vitals dashboard showing "LCP degrading" while user complaint appears on screen.  
**[Audio]** "You've probably been here: your Core Web Vitals dashboard shows LCP is degrading, but you can't figure out what's actually causing it for specific users or which part of your page is the culprit."

**[Visual]** Text overlay with telemetry types: Traces, Metrics, Logs, Events. Icons illustrate each.  
**[Audio]** "Let's talk telemetry. Each type gives you a different lens into your application — and traces might just solve problems you didn't know you could solve."

**[Visual]** Metric: Graph showing Core Web Vitals (LCP, CLS, FID) over time with P75 values highlighted, segmented by desktop/mobile.  
**[Audio]** "You're tracking Core Web Vitals — LCP, CLS, and FID — probably looking at P75 values segmented by desktop and mobile. But when your LCP spikes, which users? Which pages? Which elements caused it? Traditional metrics aggregate away the context you need to actually fix the problem."

**[Visual]** Error boundary component and Sentry error report side by side.  
**[Audio]** "Error boundaries and crash reports catch when things break, but they usually just tell you 'something went wrong' — not the full context of what the user was doing when it happened."

**[Visual]** Trace: Honeycomb UI showing a full trace with frontend and backend spans.  
**[Audio]** "Traces are different. A trace is a complete record of one user's journey through your application — when Sarah clicks 'Go' and gets a spinner that never ends, you can see exactly where it broke."

**[Visual]** Span anatomy breakdown: span ID, name, start time, duration, attributes like user.id and page URL.  
**[Audio]** "A span is one unit of work within that trace — like a click, a render, or an API call. Each span captures when it started, how long it took, and key context about what was happening."

**[Visual]** Browser dev events labeled: click, DOMContentLoaded, fetch. Mapping arrows to spans.  
**[Audio]** "When you use the Honeycomb Web SDK, those familiar browser events are automatically transformed into spans using OpenTelemetry."

**[Visual]** Honeycomb trace view: click → documentLoad → fetch → backend.  
**[Audio]** "Those spans are linked together by context — and become a trace that tells Sarah's complete story, from her button click to the server's response."

**[Visual]** JSON payload of telemetry: `name`, `startTime`, `duration`, `attributes`. Transition to span in Honeycomb.  
**[Audio]** "Behind every span is raw telemetry data — JSON payloads with timestamps, field names, and context. Honeycomb turns that into a visual timeline you can search, filter, and explore."

**[Visual]** Side-by-side: left shows Core Web Vitals dashboard with LCP spike; right shows the corresponding trace with clear bottleneck pointing to specific DOM element.  
**[Audio]** "Your Core Web Vitals dashboard tells you LCP is degrading, but a trace shows you it's specifically the meme images loading slowly on your gallery page — because the image optimization service is timing out for users in Asia."

**[Visual]** Span detail panel showing structured telemetry attributes with user context.  
**[Audio]** "This is why structured telemetry matters — instead of diving into browser DevTools or trying to reproduce issues locally, you can ask questions like 'show me all slow meme generations for users on Chrome with ad blockers enabled.'"

**[Visual]** Developer looking skeptical, then seeing a clear trace that solves their problem.  
**[Audio]** "I know what you're thinking — 'we already have error tracking and performance monitoring.' But traces give you something those tools can't: the full context of what one specific user experienced, from their browser environment to your backend."

**[Visual]** Trace includes logs inside a span and a metric attached as an attribute.  
**[Audio]** "And in Honeycomb, spans can include logs and metrics, too — so you can correlate across all three types in one view."

**[Visual]** Diagram: browser → OpenTelemetry Web SDK → Honeycomb exporter → trace view.  
**[Audio]** "The Honeycomb Web SDK uses OpenTelemetry to collect and send telemetry from the browser — turning raw events into spans with minimal code changes."

**[Visual]** Frontend span connecting to backend spans with trace context highlighted.  
**[Audio]** "Here's where it gets powerful for frontend engineers — your span doesn't end when the fetch() call starts. It connects to what happens next on the backend."

**[Visual]** Highlight trace context: `trace_id`, `span_id`, `service.name`, user-defined fields.  
**[Audio]** "Each event carries trace context — so when Sarah's button click triggers backend processing, they stitch together into a single trace. That's fullstack tracing."

**[Visual]** Complete user journey trace showing frontend click → API call → database query → response → DOM update.  
**[Audio]** "For frontend engineers, that means you can see what happened from the moment a user clicked — through your JavaScript, across the network, into the backend, and back to the DOM update."

**[Visual]** Bullet points appearing on screen with checkmarks:
• Traces give you one user's complete journey  
• Spans capture each step with full context  
• Structured telemetry lets you ask specific questions  
• Frontend and backend connect into one story  
**[Audio]** "Let's recap: Traces give you one user's complete journey through your app. Spans capture each step with full context. Structured telemetry lets you ask specific questions instead of guessing. And your frontend spans connect to backend spans — giving you the full story from click to response."

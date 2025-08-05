# Video: How Frontend Tracing Works in Honeycomb

## Learning Objectives
- Explain how frontend telemetry data becomes spans and traces
- Describe the relationship between raw telemetry data and visualizations
- Introduce trace, span, and metric data structure

---

**[Visual]** Split screen. In Query Builder WHERE clause: `name starts-with HTTP POST` > click on Traces tab and click on a Trace w/ a lot Spans (to get to Trace waterfall view).
**[Audio]** "Let's talk telemetry. Each type gives you a different lens into your application, and traces can solve problems you didn't know you could solve."

**[Visual]** Trace: Honeycomb UI showing a full trace with frontend and backend spans.
**[Audio]** "Traces can help you answer those questions. A trace is a complete record of a single interaction a user has with your application. Jo, a frontend developer who's working on Meminator—an app that generates memes—knows that, sometimes, when a user enters a phrase and clicks 'GO', they get a broken picture. With full-stack tracing, Jo can see exactly where her app crashes."

**[Visual]** Span anatomy breakdown: span ID, name, start time, duration, attributes like user.id and page URL.  
**[Audio]** "A span is one unit of work within that trace — like a click, a render, or an API call. Each span captures when it started, how long it took, and key context about what was happening."

**[Visual]** Browser dev events labeled: click, document load, fetch. Mapping arrows to spans.  
**[Audio]** "When you instrument with the Honeycomb Web SDK, those familiar browser events are automatically transformed into spans using OpenTelemetry."

**[Visual]** Honeycomb trace view: click → documentLoad → fetch → backend.  
**[Audio]** "Those spans are linked together by context — and become a trace that tells a user's complete story, from a button click to the server's response."

**[Visual]** JSON payload of telemetry: `name`, `startTime`, `endTime`, `attributes`. Side-by-side to span in Honeycomb.  Note: We derive duration by subtracting `startTime` from `endTime`.
**[Audio]** "Behind every span is raw telemetry data, exported as OTLP: JSON payloads with timestamps, field names, and context. Honeycomb turns that into a visual timeline you can search, filter, and explore."

**[Visual]** Span detail panel showing structured telemetry attributes with user context. Go to View Events in Trace. 
**[Audio]** "This is why structured telemetry matters: instead of diving into browser DevTools or trying to reproduce issues locally, you can ask questions like 'show me all slow meme generations for users on Chrome on a mobile phone.'"

**[Visual]** Jo, our developer, looking skeptical, then seeing a clear trace that solves their problem.  
**[Audio]** "I know what you're thinking: 'we already have error tracking and performance monitoring.' But traces give you something those tools can't: the full context of what one specific user experienced, from their browser environment to your backend."

**[Visual]** Diagram: browser → Honeycomb Web SDK → OTel exporter → OTLP (on arrow) → trace view.  
**[Audio]** "The Honeycomb Web SDK uses OpenTelemetry to collect and send telemetry from the browser, turning raw events into spans for page loads, user events, API calls, and Core Web Vitals."

**[Visual]** Frontend span connecting to backend spans with trace context highlighted.  
**[Audio]** "Here's where it gets powerful for frontend engineers: Your trace doesn't end when the fetch call starts. It connects to what happens next on the backend."

**[Visual]** Highlight trace context: `trace_id`, `span_id`, `service.name`, user-defined fields.  
**[Audio]** "Each event carries trace context, so when the user's button click triggers backend processing, they are stitched together into a single trace. That's end-to-end tracing."

**[Visual]** Bullet points appearing on screen with checkmarks:
• Traces give you one user's complete journey for each request 
• Spans capture each step with full context  
• Structured telemetry lets you ask specific questions  
• Frontend and backend are part of the same story, represented by a trace  
**[Audio]** "Let's recap: Traces give you one user's complete journey for each click event. Spans capture each step with full context. Structured telemetry lets you ask specific questions instead of guessing. And your frontend spans connect to backend spans — giving you the full story from click to response."

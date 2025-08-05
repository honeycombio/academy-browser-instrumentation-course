# Video: Can't See Your Full Trace? How Context Propagation Works

## Learning Objectives:
 - Diagnose missing trace segments or disconnected spans
- Explain what `traceparent` and `baggage` headers do and why they matter
- Verify and troubleshoot context propagation from browser to backend

### [Visual] Title screen: "Can't See Your Full Trace? How Context Propagation Works"
**[Audio]**
Jess is feeling great: her browser app, Meminator, is instrumented with the Honeycomb Web SDK and OpenTelemetry.
She’s seeing frontend spans in Honeycomb and getting visibility into user interactions, fetches, and document loads.
But now she’s hitting a wall.
Some traces look disconnected. Some spans start in the backend with no link to the frontend. She's wondering why the all the pieces aren't stitching together, as expected.

### [Visual] Honeycomb UI showing traces missing frontend segments
**[Audio]**
This could be a context propagation problem.
Even though spans are being collected, they won’t be stitched into a single trace unless they share a common trace ID, and that depends on the right headers being passed.

### [Visual] Highlight `traceparent` and `baggage` headers in Honeycomb span attributes table
**[Audio]**
When the frontend sends a request — like a fetch — the Honeycomb Web SDK uses OpenTelemetry's W3C Trace Context format.
That means each request includes a `traceparent` header with the trace and span ID, and a `baggage` header with metadata like the session ID.
These headers make it possible for the backend to continue the trace, but only if they’re actually received.

### [Visual] Jess clicks into a disconnected trace in Honeycomb and sees missing `trace.parent_id`
**[Audio]** 
In Honeycomb, Jess spots the issue: some backend spans don’t show a parent span ID. They aren’t part of the same trace.
The likely cause? The headers didn’t make it from the browser to the backend.


### [Visual] List of common causes in a Honeycomb query result:
- `useTraceHeaders` not enabled
- CORS blocking headers
- Backend doesn’t extract context from incoming requests
**[Audio]**
There are a few common reasons this might happen:
First, the Honeycomb Web SDK needs to have `useTraceHeaders: true` in its config. Without that, no headers are added.
Second, the backend server must allow those headers using CORS rules, or the browser will strip them.
Third, the backend must be configured to extract context from those headers and continue the trace.

### [Visual] Jess opens SDK config
**[Audio]** 
Jess opens her SDK config and sees the problem: `useTraceHeaders` was missing.

### [Visual] Updated config:

```js
new WebSDK({
  serviceName: 'insert-service-name',
  apiKey: '<your-api-key>',
  dataset: 'insert-dataset-name',
  debug: true
  useTraceHeaders: true
}).start()
```

_Note: The values need to be updated here._

**[Audio]**
She updates her config to include it.
Now, outgoing requests will automatically include `traceparent` and `baggage` headers.


### [Visual] Jess checks backend CORS policy
**[Audio]**
Next, Jess checks her backend CORS settings. She adds support for the trace headers:

### [Visual] CORS headers appear in backend code:
```http
Access-Control-Allow-Headers: traceparent, baggage
Access-Control-Allow-Origin: https://meminator.app
```
**[Audio]**
This ensures the browser can send trace context headers across origins without them being blocked.

### [Visual] Jess checks Honeycomb and still sees disconnected spans
**[Audio]** 
Still not seeing connected traces?
It’s time to check the backend instrumentation. The service must extract trace context from incoming requests and continue the trace.

### [Visual] Honeycomb UI: example of working trace with frontend → backend connection
**[Audio]**
In OpenTelemetry, this means the backend must read the incoming `traceparent` header and create new spans that share the same trace ID.
Once that’s in place, the trace flows cleanly from browser to backend.

### [Visual] Side-by-side: Broken trace vs. connected trace in Honeycomb UI
**[Audio]**
You can think of context propagation like a Swiss Army knife. If you want all your tools in one place: page load, API call, DB query, you need to pass the trace context along at every hop.
If something drops it, Honeycomb can’t stitch the picture back together.


### [Visual] Jess refreshes the app, generates a trace, and opens it in Honeycomb
**[Audio]**
Now Jess sees a full trace: click to fetch to database, all connected.

### [Visual] Final screen: "Recap"
**[Audio]**
Let’s recap what Jess did to fix her broken traces:
- She verified whether `traceparent` and `baggage` headers were present in requests
- She confirmed her Honeycomb SDK config included `useTraceHeaders: true`
- She updated backend CORS settings to allow those headers
- She checked that her backend was instrumented to extract and continue the trace context
With those pieces in place, Jess unlocked full end-to-end trace visibility.


### [Visual] Final screen: "When in doubt, check your headers"
**[Audio]** "
When your trace looks incomplete, start with the headers.
`traceparent` connects your spans. `baggage` adds context.
They’re the glue that holds your trace together."

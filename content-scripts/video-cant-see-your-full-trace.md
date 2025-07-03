# Video: Can't See Your Full Trace? How Context Propagation Works

## Learning Objectives
- Diagnose missing trace segments or disconnected spans
- Explain what `traceparent` and `baggage` headers do and why they matter
- Verify and troubleshoot context propagation from browser to backend

---

### [Visual] Title screen: "Can't See Your Full Trace? How Context Propagation Works"
**[Audio]**  
Jess is feeling great — her browser app, Meminator, is instrumented with the Honeycomb Web SDK.  
She’s seeing frontend spans in Honeycomb and getting visibility into user interactions, fetches, and document loads.  
But now she’s hitting a wall.  
Some traces look disconnected. Some spans start in the backend with no link to the frontend.  
Her question: why aren’t all the pieces stitching together?

---

### [Visual] Honeycomb UI showing traces missing frontend segments
**[Audio]**  
This could be a context propagation problem.  
Even though spans are being collected, they won’t be stitched into a single trace unless they share a common trace ID — and that depends on the right headers being passed.

---

### [Visual] Highlight `traceparent` and `baggage` headers in DevTools
**[Audio]**  
When a frontend sends a request — like a fetch — the Honeycomb Web SDK uses OpenTelemetry's W3C Trace Context format.  
This includes the `traceparent` header, which carries the trace ID and span ID, and the `baggage` header, which can carry user-defined metadata like session IDs.

---

### [Visual] Jess inspects a fetch request and notices headers are missing
**[Audio]**  
If you’re not seeing full traces, check DevTools.  
If `traceparent` or `baggage` are missing, it means context isn’t being propagated correctly — and the trace will be fragmented.

---

### [Visual] Text list: Common causes of missing headers
- `useTraceHeaders` is not set to true  
- The request is cross-origin and blocked by CORS  
- The backend does not support or forward context headers

**[Audio]**  
Here are the usual suspects:  
First, `useTraceHeaders` might not be enabled in your SDK config.  
Second, cross-origin requests may be blocked or stripped by the browser unless your server allows the trace headers using CORS.  
Third, your backend must be instrumented with OpenTelemetry or another compatible system to read and forward the headers correctly.

---

### [Visual] Jess opens her SDK config file
**[Audio]**  
Jess checks her SDK config and sees the issue. She forgot to set `useTraceHeaders` to true.

---

### [Visual] SDK config fix
```js
new WebSDK({
  serviceName: 'meminator-frontend',
  apiKey: '<your-api-key>',
  dataset: 'frontend',
  debug: true,
  useTraceHeaders: true
}).start();
```

**[Audio]**
She updates the config — now trace context headers will be automatically added to every outbound request.

---

### [Visual] Jess opens her backend server config
**[Audio]**  
She also checks her backend server.  
Since Meminator’s backend runs on a different origin, she adds CORS rules to allow the headers.

---

### [Visual] Backend CORS config
```http
Access-Control-Allow-Headers: traceparent, baggage
Access-Control-Allow-Origin: https://meminator.app
```

**[Audio]**
This ensures the browser won’t strip traceparent and baggage from fetch requests, and allows them to reach the backend.

---

### [Visual] Jess checks backend instrumentation
**[Audio]**  
Finally, Jess confirms her backend is instrumented with OpenTelemetry.  
It's configured to extract context from incoming requests, continue the trace, and propagate context to downstream services.

---

### [Visual] Jess refreshes the app and views a trace in Honeycomb
**[Audio]**  
Now she sees it — a complete trace. Click to fetch to backend — all stitched together.

---

### [Visual] Final screen: "Recap"
**[Audio]**  
Let’s recap what Jess did to fix her broken traces:  
- She verified whether `traceparent` and `baggage` headers were present in fetch requests using DevTools.  
- She confirmed her Honeycomb Web SDK config included `useTraceHeaders: true`.  
- She updated backend CORS settings to allow trace headers and origin.  
- She verified that her backend was properly instrumented with OpenTelemetry and extracting headers from requests.  
With these fixes in place, she restored complete trace visibility from frontend to backend.

---

### [Visual] Final screen: "When in doubt, check your headers"
**[Audio]**  
When your trace looks incomplete, start with the headers.  
`traceparent` connects your spans. `baggage` adds context.  
They’re the glue that holds your trace together.

# Video: Can't See Your Full Trace?

## Learning Objectives:
- Diagnose missing trace segments or disconnected spans
- Explain what `traceparent` headers do and why they matter
- Verify and troubleshoot context propagation from browser to backend

### [Visual] Title screen: "Can't See Your Full Trace? How Context Propagation Works" and Jess, our FE engineer, looking at some connected traces and some disconnected traces.
**[Audio]**
Jess is feeling great: her browser app, Meminator, is instrumented with the Honeycomb Web SDK and OpenTelemetry. She’s seeing frontend spans in Honeycomb and getting visibility into user interactions, fetches, and document loads.

But now she’s hitting a wall. Some traces look disconnected. Some spans start in the backend with no link to the frontend. She's wondering why the all the pieces aren't stitching together, as she expected.

If you're seeing disconnected traces, the problem is usually one of these: your trace propagation isn't set up correctly, CORS is blocking the necessary headers, your backend isn't extracting the trace context from incoming requests, or async requests are losing context.

Let's start with investigating context propagation.

### [Visual] Jess clicks into a disconnected trace in Honeycomb and sees missing `trace.parent_id`, then highlight `traceparent` headers in SDK
**[Audio]**
In order for traces to propagate correctly, here's how it should work: when your frontend makes a request using fetch, Honeycomb's Web SDK automatically adds a traceparent header following OpenTelemetry's W3C Trace Context standard. This header carries the trace and span IDs that allow your backend to continue the same trace.

But this only works if those headers actually make it to your backend.

### [Visual] Code snippet with `enabled: false` highlighted on both packages
**[Audio]**
This might happen if you're not using OpenTelemetry's standard instrumentation for `fetch` or `XMLHttpRequest`. Honeycomb's SDK enables this by default, but if you've manually disabled it, you'll break trace propagation.

Here's what the problematic config looks like. Notice that the instrumentation is disabled here.

```js
const sdk = new HoneycombWebSDK({
  instrumentations: [
    getWebAutoInstrumentations({
      // this is what it looks like when they disable the instrumentation:
      "@opentelemetry/instrumentation-xml-http-request": {
        enabled: false,
      },
      "@opentelemetry/instrumentation-fetch": {
        enabled: false,
      },
      // ^^^^
    }),
    new UserInteractionInstrumentation(),
  ],
  serviceName,
});
```

**[Audio]**
Instead, stick with the default configuration.

```js
const sdk = new HoneycombWebSDK({
  instrumentations: [
    getWebAutoInstrumentations(),
    new UserInteractionInstrumentation(),
  ],
  serviceName,
});
```

**[Audio]** Show SDK with code snippet for `propagateTraceHeaderCorsUrls`
Now, even if your instrumentation is configured correctly, you can still run into issues if CORS is blocking the trace headers from reaching your backend.

When CORS is blocking your trace headers, there are usually three things to check. 
- First, make sure Honeycomb's SDK is actually configured to send headers to your backend using the `propagateTraceHeaderCorsUrls` option. 
- Second, your backend needs to explicitly allow these headers through its CORS policy, or the browser will strip them out. 
- And third, your backend instrumentation needs to be set up to extract and use that trace context.

Let's start with the frontend config. If this regex doesn't match your backend URL, the SDK won't include the traceparent header at all.

```js
const sdk = new HoneycombWebSDK({
  serviceName,
  propagateTraceHeaderCorsUrls: [/* insert regex to match your backend */]
});
```

### [Visual] Jess checks backend CORS policy, show backend config
**[Audio]**
Next, Jess needs to check her backend's CORS configuration. The server needs to explicitly allow the trace headers:

```http
Access-Control-Allow-Headers: traceparent, baggage
Access-Control-Allow-Origin: https://meminator.app
```

**[Audio]**
These CORS rules tell the browser it's safe to send trace context headers across origins, preventing them from being blocked.

### [Visual] Jess checks Honeycomb and still sees disconnected spans
**[Audio]** 
If you're still seeing disconnected traces after checking instrumentation and CORS, it's time to look at your backend. 

### [Visual] Honeycomb UI: example of working trace with frontend → backend connection and and backend instrumentation code with correct config
**[Audio]**
Here's what should happen: your backend reads the traceparent header from the incoming request and creates new spans using the same trace ID. If you're using standard OpenTelemetry instrumentation for your backend—whether that's Go, Java, Python, or another language—this context extraction should happen automatically with the right configuration.

Once everything is wired up correctly, you'll see traces that flow seamlessly from your browser all the way through your backend services.

### [Visual] Honeycomb UI: show disconnected trace with just the click 
**[Audio]** Now that we've covered frontend-to-backend trace propagation, let's look at a different issue: traces that break within the frontend itself.

Sometimes you'll see trace breaks that happen entirely on the frontend. For example, a user clicks a button that triggers an HTTP request, but in Honeycomb these appear as separate, disconnected traces instead of a single flow.

This typically happens because OpenTelemetry can't automatically maintain trace context across async operations. When you make fetch or XMLHttpRequest calls inside async functions, the trace context can get lost.

### [Visual] Code showing async function making fetch request
**[Audio]**
The solution is to use OpenTelemetry's Zone Context Manager, which tracks context across async boundaries:

```js
const sdk = new HoneycombWebSDK({
  // other config options omitted...
  contextManager: new ZoneContextManager()
});
sdk.start();
```

### [Visual] Honeycomb showing connected trace: button click → HTTP request → backend
**[Audio]**
With this in place, you'll see the complete story: the button click span connects to the HTTP request span, which then flows into your backend spans, giving you end-to-end visibility of the user's action.

[note: see README: https://github.com/honeycombio/honeycomb-opentelemetry-web/tree/main/packages/honeycomb-opentelemetry-web#context-management]

### [Visual] Slide w/ key takeaways
**[Audio]**
Let's recap the ways you can fix disconnected traces.
- Work through each possibility methodically: frontend instrumentation, CORS, backend extraction, async context.
- Don't disable fetch instrumentation. Keep `@opentelemetry/instrumentation-fetch` and `@opentelemetry/instrumentation-xml-http-request` enabled in your SDK config.
- Look for spans without `trace.parent_id` in Honeycomb to spot propagation breaks.
- Configure CORS properly. Set `propagateTraceHeaderCorsUrls` on the frontend and allow `traceparent` headers on your backend.
- Verify backend extraction. Ensure your backend OpenTelemetry instrumentation reads incoming trace headers automatically.
- Use `ZoneContextManager` for async. Add it to your SDK config to maintain trace context across async operations.

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

### [Visual] Jess clicks into 2 disconnected traces in Honeycomb, then highlight `traceparent` headers in SDK
**[Audio]**
In order for traces to propagate correctly, here's how it should work: when your frontend makes a request using fetch, Honeycomb's Web SDK automatically adds a traceparent header following OpenTelemetry's W3C Trace Context standard. This header carries the trace and span IDs that allow your backend to continue the same trace.

But this only works if those headers actually make it to your backend.

### [Visual] Code snippet with `enabled: false` highlighted on both packages
**[Audio]**
This might happen if you're not using OpenTelemetry's standard instrumentation for `fetch` or `XMLHttpRequest`. Honeycomb's SDK enables the instrumentation by default, but if you've manually disabled it, you won't see the spans for your web dataset.

Here's what a problematic config looks like. Notice that the instrumentation is disabled here.

```js
const sdk = new HoneycombWebSDK({
  apiKey: '[YOUR API KEY HERE]', 
  serviceName: '[YOUR APPLICATION NAME HERE]', 
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
});
```

**[Audio]** Show SDK with code snippet for `propagateTraceHeaderCorsUrls`
Next, we need to configure the endpoint patterns that will recieve the `traceparent` header. 
You may run into issues if the Honeycomb Web SDK is blocking the `traceparent` header from reaching your backend.

In this case, there are usually three things to check. 
- First, make sure Honeycomb's Web SDK is actually configured to send headers to your backend using the `propagateTraceHeaderCorsUrls` option for the fetch/XMLHttpRequest/documentLoad instrumentation. [Visual: Highlight config defaults block and getWebAutoInstrumentations block]
- Second, your backend needs to explicitly allow these headers through its CORS policy, or it will strip them out or possibly fail because it refuses them.
- And third, your backend instrumentation needs to be set up to extract and use that trace context.

Let's start with the frontend config. If the regex patterns don't match your backend URL, the SDK won't include the traceparent header at all.

```js
const configDefaults = {
  propagateTraceHeaderCorsUrls: [
        // this regexp means add to all outgoing fetch/XMLHttpRequest calls
        /.*/g,
        // this regexp allows APIs to a specific host - note the escape characters before the forward slashes
        /http:\/\/api.meminator.org:8080/
    ]
};

const sdk = new HoneycombWebSDK({
  apiKey: '[YOUR API KEY HERE]', 
  serviceName: '[YOUR APPLICATION NAME HERE]', 
  instrumentations: [
    getWebAutoInstrumentations({
      '@opentelemetry/instrumentation-xml-http-request': configDefaults,
      '@opentelemetry/instrumentation-fetch': configDefaults,
      '@opentelemetry/instrumentation-document-load': configDefaults,
      ...
    new UserInteractionInstrumentation(),
    }),
  ],
  serviceName,
});
```

### [Visual] Jess checks backend CORS policy, show the code snippet
**[Audio]**
Next, Jess needs to check her backend's CORS configuration. The server needs to explicitly allow the `traceparent` and `tracestate` headers:

```http
Access-Control-Allow-Headers: traceparent, tracestate
Access-Control-Allow-Origin: https://meminator.app
```

This example allows only CORS headers if the app was running on the website https://meminator.app.

**[Audio]**
These CORS rules tell the browser it's safe to send trace context headers across origins, preventing them from being blocked.

### [Visual] Jess checks Honeycomb and still sees disconnected spans
**[Audio]** 
If you're still seeing disconnected traces after checking frontend instrumentation and CORS, it's time to look at your backend. 

### [Visual] Honeycomb UI: example of working trace with frontend → backend connection and and backend instrumentation code with correct config
**[Audio]**
Here's what should happen: your backend reads the traceparent header from the incoming request and creates new spans using the same trace ID. If you're using standard OpenTelemetry instrumentation for your backend this context extraction should happen automatically with the right configuration.

Once everything is wired up correctly, you'll see traces that flow seamlessly from your browser all the way through your backend services.

### [Visual] Honeycomb UI: show disconnected trace with just the click 
**[Audio]** Now that we've covered frontend-to-backend trace propagation, let's look at a different issue: traces that are disconnected within the frontend itself.

Sometimes you'll see disconnected traces that happen entirely on the frontend. For example, a user clicks a button that triggers an HTTP request, but in Honeycomb these appear as separate, disconnected traces instead of a single flow.

This typically happens because OpenTelemetry can't always maintain trace context across async operations. When you make fetch or XMLHttpRequest calls inside async functions, the trace context can get lost.

### [Visual] Code 
**[Audio]**
One method you could try is to use OpenTelemetry's Zone Context Manager, which tracks context across async boundaries:

```js
const sdk = new HoneycombWebSDK({
  // other config options omitted...
  contextManager: new ZoneContextManager()
});
sdk.start();
```

You may see the complete story in one trace here, but it's not guaranteed. Also, this library adds a significant amount of JavaScript to your browser's payload. As another option, you could query in Honeycomb by `session.id` and view the events in ascending date order.

### [Visual] Honeycomb showing connected trace: button click → HTTP request → backend
**[Audio]**

In some cases, you'll see the complete story: the button click span connects to the HTTP request span, which then flows into your backend spans, giving you end-to-end visibility of the user's action.

[note: see README: https://github.com/honeycombio/honeycomb-opentelemetry-web/tree/main/packages/honeycomb-opentelemetry-web#context-management]

### [Visual] Slide w/ key takeaways
**[Audio]**
Let's recap the ways you can fix disconnected traces.
- Work through each possibility methodically: frontend instrumentation, CORS, backend extraction, async context.
- Don't disable fetch or XMLHttpRequest instrumentation. Keep `@opentelemetry/instrumentation-fetch` and `@opentelemetry/instrumentation-xml-http-request` enabled in your SDK config.
- Configure CORS properly. Set `propagateTraceHeaderCorsUrls` on the frontend and allow `traceparent` headers on your backend.
- Verify backend extraction. Ensure your backend OpenTelemetry instrumentation reads incoming trace headers automatically.
- Try using `ZoneContextManager` for async. Add it to your SDK config to maintain trace context across async operations. Or, you can try querying by `session.id` in Honeycomb and viewing your events.

# Video: Automatic Instrumentation with Honeycomb Web SDKs + OpenTelemetry

## Learning Objectives
- Explain how automatic instrumentation reduces setup effort
- Verify whether automatic instrumentation is working correctly
- Describe types of telemetry automatically collected
- Emphasize the need to double-check for trace propagation headers and baggage for session ID

---

Notes:
- Remember, they have to run `npm install` in their React folder first.
- FE developers want to run and stop in development mode.
- Consider various methods for importing API keys.

---

### [Visual] Title screen: "Automatic Instrumentation: See More with Less Setup"  
**[Audio]**  
Meet Jess, a frontend engineer who is working on a web application called Meminator. She wants to see full-stack traces in Honeycomb.  
She's looking for a way to start capturing meaningful telemetry with as little custom setup as possible.  
Jess can already see backend traces; she knows when requests hit her APIs, how long database queries take, and where backend services interact.  
But she’s missing the start of the story. Without frontend spans, she can’t trace user interactions that trigger those requests; she doesn’t know if the button click even registered, how long the browser stalled, or whether a third-party script delayed the request.  
She wants to see the full trace — from a user click to DB query — and get the full picture.

In this video, you'll learn how to add automatic instrumentation to a browser application and see what you get from it. 

---
### [Visual] Jess runs the browser app, Meminator, to make confirm its expected behavior. Run `./run` in the terminal in the root folder. Show Meminator's behavior: `GO` button and create-your-own-phrase bar. Then run `./stop`.
**[Audio]**
Before we get started, Jess runs Meminator to confirm its expected behavior. Ah! Looks like we can generate memes. That's what Meminator is supposed to do. Let's stop the application, then add automatic instrumentation.

### [Visual] Jess installs the required Honeycomb and OTel packages in `services/react/`. 
```bash
npm install @honeycombio/opentelemetry-web @opentelemetry/auto-instrumentations-web
```
**[Audio]**
First, Jess installs the Honeycomb and OpenTelemetry packages in the entry file using the terminal. 
These packages included everything needed to automatically capture frontend telemetry and forward it to Honeycomb.  The Honeycomb SDK wraps the OpenTelemetry JavaScript SDK and will set up common instrumentation out of the box, saving time and reducing the chance of manual error.

---

### [Visual] Code snippet for import statements in `services/react/src/main.tsx`: 
```js
import { HoneycombWebSDK } from '@honeycombio/opentelemetry-web';
import { getWebAutoInstrumentations } from '@opentelemetry/auto-instrumentations-web';
```
**[Audio]**  
After installing the packages, Jess imports the `WebSDK` and `opentelemetry-web` classes into the main entry point of her application. These import statements go at the very top of the file. This gives her access to the initializer that starts capturing and exporting spans.

---

### [Visual] SDK configuration appears  
```js
const sdk = new HoneycombWebSDK({
    apiKey: 'your ingest api key',
    serviceName: 'react',
    instrumentations: [
        getWebAutoInstrumentations()
    ]
});
sdk.start();
```
**[Audio]**
Next, Jess is wires up automatic instrumentation by configuring the Honeycomb Web SDK.

She creates a new SDK instance and passes in three things:

First, her Honeycomb API key: this is what authenticates her telemetry with Honeycomb’s ingest API.

Second, a service name: she labels this frontend app as "react". That name will appear in Honeycomb when she views her spans or traces, and helps her tell different services apart.

Third, she provides a list of instrumentations using getWebAutoInstrumentations().
This is a helper from OpenTelemetry that enables automatic span creation for things like page loads, fetch requests, user interactions, Web Vitals, and more, all without having to write manual code.

Finally, she calls `sdk.start()` to activate the SDK. That’s what kicks off telemetry collection and sends her spans to Honeycomb.

Once this is in place, Jess can refresh her app, open Honeycomb, start seeing new spans for a handful of things: document loads, fetch requests, Core Web Vitals, and more.

This step is where the instrumentation actually begins, and it's what unlocks visibility into what Meminator users are experiencing in the browser.

### [Visual] Start Meminator back up, run `./run` in terminal.
**[Audio]**
Let's run Meminator again, then validate the results in Honeycomb.

### [Visual] Highlighting traces and spans in Honeycomb UI Web Launchpad
**[Audio]**
As soon as the SDK starts, Jess sees spans showing up in Honeycomb: no custom code required.
To make sure we've wired it up correctly, let's make sure we're in the correct dataset. In this example, we should be in the `react` dataset.
The SDK automatically instruments key parts of her app using OpenTelemetry defaults.
This includes:
- Page loads and navigation events
- Core Web Vitals like Largest Contentful Paint and First Input Delay
- Click events and user interactions
- Fetch and XHR requests
- JavaScript errors
- Browser and device metadata

### [Visual] Attribute table: user agent, page location, HTTP method, status code, resource type
**[Audio]**
Let's see what information we can get with our spans. Each span includes rich attributes automatically populated with context like:
- The full page URL
- HTTP method and status
- Resource timing metrics
- Browser type and version
- Feature flag status
- All of this uses OpenTelemetry semantic conventions — which makes it easier to query consistently.

### [Visual] Trace waterfall showing frontend spans connected to backend spans
**[Audio]**
Within those spans, she inspects key attributes that confirm proper setup:
- trace.trace_id and trace.parent_id, showing correct trace context.
- Attributes like http.method, http.url, user_agent, and lcp, aligned with OpenTelemetry semantic conventions.
- Honeycomb‑level derived fields such as is_root and session metadata, confirming session IDs are carried as baggage.

Now, the best part: we can see that frontend spans are connected to backend traces.
The SDK uses the W3C Trace Context format to pass trace headers along with fetch requests.
If the backend is also instrumented with OpenTelemetry, the trace continues seamlessly across services.

### [Visual] Show session.id in Honeycomb baggage
**[Audio]**
By default, the SDK generates a new session.id and attaches it to every span via the baggage header.
This lets Jess filter traces by user session in Honeycomb.
If needed, she can override the session logic using a custom session ID; for example, based on authentication.

### [Visual] Honeycomb UI showing complete trace
**[Audio]**
Now Jess can see everything from the user’s click to the database query, all in one trace!
Automatic instrumentation helped her get started fast, with minimal boilerplate and maximum insight.

### [Visual] Final screen: “Key Takeaways”
Let’s recap what Jess got from automatic instrumentation:
- Key browser events and API calls were instrumented out of the box
- Span attributes gave her rich context with zero config
- Trace context was passed automatically to backend services
- She could override the session ID logic when needed

All of that, without writing any custom instrumentation code!

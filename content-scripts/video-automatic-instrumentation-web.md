# Video: Automatic Instrumentation with Honeycomb Web SDKs + OpenTelemetry

## Learning Objectives
- Explain how automatic instrumentation reduces setup effort
- Verify whether automatic instrumentation is working correctly in Honeycomb
- Explain the types of telemetry and attributes automatically collected
- Emphasize the need to double-check for trace propagation headers and session IDs

---

### [Visual] Title screen: "Browser Automatic Instrumentation with Honeycomb WebSDK and OTel"  
**[Audio]**  
Meet Jess, a frontend engineer who is working on a web application called Meminator. 
She's looking for a way to start capturing meaningful telemetry with as little custom setup as possible.  
Jess can already see backend traces; she knows when requests hit her APIs, how long database queries take, and where backend services interact.  
But she’s missing the start of the story. Without frontend spans, she can’t trace user interactions that trigger those requests; she doesn’t know if the button click even registered, how long the browser stalled, or whether a third-party script delayed the request.  
She wants to see end-to-end traces — from the intial user interaction to database query — and get the full picture.

In this video, you'll learn how to add automatic instrumentation to a browser application and verify it's working correctly by viewing the resulting end-to-end traces in Honeycomb. 

---
### [Visual] Jess runs the browser app, Meminator, to make confirm its expected behavior. Run `./run` in the terminal in the root folder. Show Meminator's behavior: `GO` button and create-your-own-phrase bar. Then run `./stop`.
**[Audio]**
Before we get started, Jess runs Meminator to confirm its expected behavior. Ah! Looks like it can generate memes. That's what Meminator is supposed to do. Let's stop the application, then add automatic instrumentation.

### [Visual] Jess installs the required Honeycomb and OTel packages in `services/react/package.json`. 
```bash
cd services/react
npm install @honeycombio/opentelemetry-web @opentelemetry/auto-instrumentations-web
```
**[Audio]**
First, Jess uses `npm install` to add the Honeycomb and OpenTelemetry packages in the `package.json` file in the `services/react` folder using the terminal. 
These packages include everything needed to automatically capture frontend telemetry and forward it to Honeycomb. The Honeycomb SDK wraps the OpenTelemetry JavaScript SDK and will set up common instrumentation out of the box, saving time and reducing the chance of manual error.

---

### [Visual] Code snippet for import statements in `services/react/src/main.tsx`: 
```js
import { HoneycombWebSDK } from '@honeycombio/opentelemetry-web';
import { getWebAutoInstrumentations } from '@opentelemetry/auto-instrumentations-web';
```
**[Audio]**  
After installing the packages, Jess imports the `HoneycombWebSDK` class and `getWebAutoInstrumentations` function into the main entry point of her application. These import statements go at the very top of the file. This gives her access to the initializer that starts capturing and exporting spans.

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
// React createRoot goes here
```
**[Audio]**
Next, Jess wires up automatic instrumentation by configuring the Honeycomb Web SDK.

She creates a new SDK instance and passes in three things:

First, her Honeycomb API key: this is what authenticates her telemetry with Honeycomb’s ingest API.

Second, a service name: she labels this frontend app as "react". That name will appear in Honeycomb when she views her spans or traces, and helps her tell different services apart.

Third, she provides a list of instrumentations using `getWebAutoInstrumentations()`.
This is a helper function from OpenTelemetry that enables automatic span creation for things like page loads, fetch requests, user interactions, Core Web Vitals, and more, all without having to write manual code.

Finally, she calls `sdk.start()` to activate the SDK. That’s what kicks off telemetry collection and sends her spans to Honeycomb.

Once this is in place, Jess can refresh her app, open Honeycomb, and start seeing new spans for a handful of things: document loads, fetch requests, Core Web Vitals, and more.

This step is where the instrumentation actually begins, and it's what unlocks visibility into what Meminator users are experiencing in the browser.

### [Visual] Start Meminator back up, run `./run` in terminal.
**[Audio]**
Let's run Meminator again, then validate the results in Honeycomb.

### [Visual] Highlighting traces and spans in Honeycomb UI Web Launchpad
**[Audio]**
As soon as the SDK starts, Jess sees spans showing up in Honeycomb.
To validate we've wired it up correctly, let's first make sure we're in the correct dataset. In this example, we should be in the `react` dataset.
The SDK automatically instruments key parts of her app using OpenTelemetry defaults.
This includes:
- Errors
- Click events and user interactions
- Fetch and XHR requests
- Page loads and navigation events
- Core Web Vitals like Largest Contentful Paint (LCPs) and First Input Delay
- Browser and device metadata

### [Visual] Go into an end-to-end trace, view of the attribute table with attributes. Use the search function to show these attributes.
**[Audio]**
Let's see what information we can get with our spans. Each span includes rich attributes automatically populated with context like:
- The full page URL
- HTTP method and status
- Resource timing
- Browser type and version

All of these uses OpenTelemetry semantic conventions, which makes it easier to query consistently.

### [Visual] Trace waterfall showing frontend spans connected to backend spans
**[Audio]**
Within those spans, she inspects key attributes that confirm proper setup:
- She looks for trace IDs and parent IDs to ensure the trace context is properly connected.
- She checks that HTTP attributes like method, URL, and user agent are all captured according to OpenTelemetry standards, along with performance metrics like `lcp` attributes.
- She confirms that session IDs are being tracked.
- She also verifies that Honeycomb's derived fields, like `is_root`, are present.

Now, the best part: we can see that frontend spans are connected to backend traces.
The SDK uses the W3C `Traceparentcontext` header to pass trace headers with fetch requests.
If the backend is also instrumented with OpenTelemetry, the trace continues seamlessly across services.

### [Visual] Show a trace with only a click, route them to [Documentation](https://docs.honeycomb.io/send-data/javascript-browser/honeycomb-distribution/?utm_source=chatgpt.com) on evaluating what interactions they should see.
**[Audio]** 
Sometimes, you’ll see a beautiful end-to-end trace that starts with the click and continues through the fetch call, the backend services, and even the database. 
Other times the browser records a button click as its own trace in Honeycomb, followed by another trace with the fetch and other end-to-end interactions. 
Both behaviors are “normal,” and the difference comes down to how JavaScript handles asynchronous events in the browser. 

### [Visual] Show session IDs in Honeycomb UI
**[Audio]**
By default, the Honeycomb SDK automatically generates session IDs and attaches them to every span - as an attribute - during a browser session. This is crucial for frontend observability. Since user interactions only live for the duration of the interaction itself, trace context is lost before the user initiates subsequent requests in the browser. Session IDs are the key linking mechanism, since these frontend interactions are oftentimes spread across many traces.
Session IDs also allow Jess to filter traces by user session in Honeycomb.

If needed, it's possibe to override the session logic using a custom session ID; for example, if you can log in as different users in your web application, you may want to generate a new session ID every time you switch users.

### [Visual] Honeycomb UI showing complete trace
**[Audio]**
With session IDs in place, Jess can now follow a user’s journey across multiple browser interactions and traces, rich browser-specific attributes, and session attributes - all in one place! 

Automatic instrumentation helped her get started fast, with minimal custom coding and lots of great insight.

### [Visual] Final screen: “Key Takeaways”
Let’s recap what Jess got from automatic instrumentation:
- Automatic instrumentation with OpenTelemetry and the Honeycomb WebSDK captures important frontend spans with no manual coding.
- Honeycomb’s Web SDK wraps OpenTelemetry to collect page loads, clicks, fetches, Core Web Vitals, and more - right out of the box.
- Make sure to verify proper header propagtion. Use the W3C `Traceparentcontext` header so frontend spans connect seamlessly to backend traces. _Note: Include a link to [CORS errors documentation](https://docs.honeycomb.io/troubleshoot/common-issues/sending-data/#cors-errors).
- Session IDs link multiple frontend traces for the same user, even when traces are split.

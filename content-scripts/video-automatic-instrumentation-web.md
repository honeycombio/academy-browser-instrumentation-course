# Video: Automatic Instrumentation with Honeycomb Web SDKs + OpenTelemetry

## Learning Objectives
- Describe types of telemetry automatically collected
- Explain how automatic instrumentation reduces setup effort
- Verify whether automatic instrumentation is working correctly
- Emphasize the need to double-check for trace propagation headers and baggage for session ID

---

Notes:
- Remember, they have to run `npm install` in their React folder first.
- FE developers want to run and stop in development mode.
- Consider various methods for importing API keys.

---

### [Visual] Title screen: "Automatic Instrumentation: See More with Less Setup"  
**[Audio]**  
Meet Jess — a frontend engineer who is working on a web application called Meminator. She wants to see full-stack traces in Honeycomb.  
She's looking for a way to start capturing meaningful telemetry with as little custom setup as possible.  
Jess can already see backend traces; she knows when requests hit her APIs, how long database queries take, and where backend services interact.  
But she’s missing the start of the story. Without frontend spans, she can’t trace user interactions that trigger those requests — she doesn’t know if the button click even registered, how long the browser stalled, or whether a third-party script delayed the request.  
She wants to see the full trace — from a user click to DB query — and get the full picture.

In this video, you'll learn how to add automatic instrumentation to a browser application and see what you get from it. 

---
### [Visual] Jess runs the browser app, Meminator, to make confirm its expected behavior. Run `./run` in the terminal in the root folder. Show Meminator's behavior: `GO` button and create-your-own-phrase bar.
**[Audio]**
Before we get started, Jess runs the application to confirm its expected behavior. Ah! Looks like we can ggenerate memes. That's what Meminator is supposed to do.

### [Visual] Jess installs the required Honeycomb and OTel packages in `services/react/`. 
```bash
npm install @honeycombio/opentelemetry-web @opentelemetry/auto-instrumentations-web
```
**[Audio]**

### [Visual] Jess installs the SDK using `npm install @honeycombio/web-sdk` in the `services/react/` folder.
**[Audio]**  
First, Jess installs the Honeycomb Web SDK package using the terminal in the entry file:  
`npm install @honeycombio/web-sdk`  
This package includes everything needed to automatically capture frontend telemetry and forward it to Honeycomb.  
It wraps the OpenTelemetry JavaScript SDK and will set up common instrumentation out of the box, saving time and reducing the chance of manual error.

---

### [Visual] Code snippet: `import { WebSDK } from '@honeycombio/web-sdk'`  
**[Audio]**  
After installing the SDK, Jess imports the `WebSDK` class into the main entry point of her application.  
This gives her access to the initializer that starts capturing and exporting spans.

---

### [Visual] SDK configuration appears  
```js
new WebSDK({
  serviceName: 'insert-service-name',
  apiKey: '<your-api-key>',
  dataset: 'insert-dataset-name',
  debug: true
}).start()
```
**[Audio]**
Jess calls new WebSDK() in the main entry point of Meminator with a few essential settings: the service name, API key, and target dataset. This should be placed before any app framework code initializes so the SDK can capture page load and early lifecycle events.
She enables debug mode so she can inspect SDK behavior in the browser console during development.

_Note: We need to modify the code snippet for the correct values and clarify where in the application this code snippet will go. In the React folder? ... in which file?_

### [Visual] Highlighting trace spans in DevTools network tab and Honeycomb UI
**[Audio]**
As soon as the SDK starts, Jess sees spans showing up in Honeycomb — no custom code required.
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
Each span includes rich attributes — automatically populated with context like:
- The full page URL
- HTTP method and status
- Resource timing metrics
- Browser type and version
- Feature flag status
- All of this uses OpenTelemetry semantic conventions — which makes it easier to query consistently.

### [Visual] Trace waterfall showing frontend spans connected to backend spans
**[Audio]**
Even better — her frontend spans are connected to backend traces.
The SDK uses the W3C Trace Context format to pass trace headers along with fetch requests.
If the backend is also instrumented with OpenTelemetry, the trace continues seamlessly across services.

### [Visual] Network tab showing request headers: traceparent, baggage
**[Audio]**
Jess opens the DevTools network tab and confirms the headers:
- traceparent, which carries the trace ID
- baggage, which carries metadata like the session ID
- These headers are added automatically — but she knows it’s best to verify that they’re showing up as expected.

### [Visual] Console.log showing session.id in Honeycomb baggage
**[Audio]**
By default, the SDK generates a new session.id and attaches it to every span via the baggage header.
This lets Jess filter traces by user session in Honeycomb.
If needed, she can override the session logic using a custom session ID — for example, based on authentication.

### [Visual] Code block: Customize session ID
```js
new WebSDK({
  sessionIdGenerator: () => {
    return localStorage.getItem('session_id') || uuidv4();
  }
})
```
**[Audio]**
Jess adds a sessionIdGenerator to make the session ID persistent between page reloads.
This makes it easier to track the user journey in full.

### [Visual] Debug log in console showing spans created
**[Audio]**
To verify that everything’s working, Jess enables debug mode.
She sees logs in the console showing when spans are started, enriched, and exported — a useful way to validate that her config is correct.

### [Visual] Honeycomb UI showing complete trace
**[Audio]**
Now Jess can see everything from the user’s click to the database query — all in one trace.
Automatic instrumentation helped her get started fast, with minimal boilerplate and maximum insight.

### [Visual] Final screen: “Key Takeaways”
Let’s recap what Jess got from automatic instrumentation:
- Key browser events and API calls were instrumented out of the box
- Span attributes gave her rich context with zero config
- Trace context was passed automatically to backend services
- She could override the session ID logic when needed
- And she verified everything using debug logs and DevTools

All of that — without writing any custom spans!

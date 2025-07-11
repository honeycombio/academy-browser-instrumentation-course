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


### [Visual] Title screen: "Automatic Instrumentation: See More with Less Setup"  
**[Audio]**  
Meet Jess — a frontend engineer who is working on a web application called Meminator. She wants to see full-stack traces in Honeycomb.  
She's looking for a way to start capturing meaningful telemetry with as little custom setup as possible.  
Jess can already see backend traces — she knows when requests hit her APIs, how long database queries take, and where backend services interact.  
But she’s missing the start of the story. Without frontend spans, she can’t trace user interactions that trigger those requests — she doesn’t know if the button click even registered, how long the browser stalled, or whether a third-party script delayed the request.  
She wants to see the full trace — from click to DB query — and get the full picture.

---

### [Visual] Jess installs the SDK using `npm install @honeycombio/web-sdk`  
**[Audio]**  
First, Jess installs the Honeycomb Web SDK package using her terminal:  
`npm install @honeycombio/web-sdk`  
This package includes everything needed to automatically capture frontend telemetry and forward it to Honeycomb.  
It wraps the OpenTelemetry JavaScript SDK and sets up common instrumentation out of the box — saving Jess time and reducing the chance of manual error.

---

### [Visual] Code block: `import { WebSDK } from '@honeycombio/web-sdk'`  
**[Audio]**  
After installing the SDK, Jess imports the `WebSDK` class into the main entry point of her application.  
This gives her access to the initializer that starts capturing and exporting spans.

---

### [Visual] SDK configuration appears  
```js
new WebSDK({
  serviceName: 'my-frontend-app',
  apiKey: '<your-api-key>',
  dataset: 'frontend',
  debug: true
}).start()

# Architecture Diagram: Honeycomb Frontend Instrumentation Overview

**Learning Objectives:**
- Identify each component in the Honeycomb Web SDK architecture (e.g., user events, OpenTelemetry Web SDK, exporter, collector)
- Trace the path of a user interaction from the browser to Honeycomb ingest
- Pinpoint where to inject custom fields, manage sessions, or secure data
- Understand where trace context originates and how it is propagated

---

## Interactive Audio Hotspots

### 1. User Interaction
**Diagram Element:** A user clicking a button on a webpage.  
**Audio:** "When a user interacts with your application—like clicking a button or navigating to a new page—these actions trigger events in the browser. The Honeycomb Web SDK captures these interactions to initiate spans that represent the start of a trace."

### 2. Honeycomb Web SDK Initialization
**Diagram Element:** JavaScript code snippet initializing the Honeycomb Web SDK.  
**Audio:** "The Honeycomb Web SDK initializes early in your application's lifecycle, setting up automatic instrumentation for various browser events. This includes capturing Core Web Vitals, document load times, and user interactions, all structured as spans."

### 3. OpenTelemetry Instrumentation
**Diagram Element:** Integration point between the Honeycomb Web SDK and OpenTelemetry libraries.  
**Audio:** "Under the hood, the Honeycomb Web SDK leverages OpenTelemetry's JavaScript libraries to standardize the collection of telemetry data. This ensures compatibility and flexibility in how data is captured and processed."

### 4. Span Creation and Context Propagation
**Diagram Element:** Flowchart showing spans being created and context (trace IDs) being propagated.  
**Audio:** "Each captured event is transformed into a span, a fundamental unit of work in tracing. The SDK manages context propagation, linking spans together using trace IDs to form a complete trace across different parts of your application."

### 5. Exporter Configuration
**Diagram Element:** Configuration settings for exporting data, including API keys and endpoints.  
**Audio:** "The exporter is configured with your Honeycomb API key and dataset information. It handles batching and sending spans to the designated endpoint, ensuring that your telemetry data reaches Honeycomb securely and efficiently."

### 6. OpenTelemetry Collector (Optional)
**Diagram Element:** An intermediary service between the browser and Honeycomb's ingest endpoint.  
**Audio:** "Optionally, you can route telemetry data through the OpenTelemetry Collector. This adds a layer of abstraction, allowing for additional processing, filtering, or routing before data reaches Honeycomb."

### 7. Honeycomb Ingest Endpoint
**Diagram Element:** Honeycomb's API endpoint receiving telemetry data.  
**Audio:** "The Honeycomb ingest endpoint receives the structured telemetry data, where it's stored and made available for querying and visualization in the Honeycomb UI."

### 8. Honeycomb UI and Querying
**Diagram Element:** Screenshot of Honeycomb's UI showing a trace view.  
**Audio:** "Within Honeycomb, you can explore traces to understand the flow of user interactions through your application. The UI provides powerful querying capabilities to dissect and analyze performance bottlenecks or errors."

### 9. Custom Instrumentation Points
**Diagram Element:** Highlighted areas in code where developers can add custom spans or attributes.  
**Audio:** "Beyond automatic instrumentation, you can add custom spans and attributes to capture domain-specific information. This enriches your traces with context that's meaningful to your application's behavior."

### 10. Session and User Context Management
**Diagram Element:** Mechanism showing how session IDs and user information are attached to spans.  
**Audio:** "Managing session and user context is crucial for correlating events. The SDK allows you to define how session IDs are generated and propagated, ensuring that related spans are grouped appropriately."

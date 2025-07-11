# Architecture Diagram: Honeycomb Frontend Instrumentation Overview

**Learning Objectives:**
- Identify each component in the Honeycomb Web SDK architecture (e.g., user events, OpenTelemetry Web SDK, exporter, collector)
- Trace the path of a user interaction from the browser to Honeycomb ingest
- Pinpoint where to inject custom fields, manage sessions, or secure data
- Understand where trace context originates and how it is propagated

---

# Interactive Audio Hotspots: Meminator Web SDK Architecture

# Interactive Audio Hotspots: Meminator Web SDK Architecture

## 1. User Interaction
**Diagram Element:** User clicking the "Go" button in the Meminator React frontend.  
**Audio:** "When a user clicks the 'Go' button or enters custom text in Meminator, these actions trigger DOM events in the browser. Think of observability as adding a 'flight recorder' to your web app—it captures what users do so you can understand how your app behaves in the real world. The Honeycomb Web SDK automatically detects these interactions and starts creating what we call 'spans'—like breadcrumbs that show the journey of each user action."

## 2. Honeycomb Web SDK Initialization  
**Diagram Element:** SDK initialization layer wrapping the Meminator React frontend component.  
**Audio:** "The Honeycomb Web SDK is like adding a monitoring system to your React app. When Meminator loads, the SDK automatically sets up instrumentation—that's a fancy word for 'watching what happens.' It tracks page loads, button clicks, API calls, and even errors. The best part? You don't need to manually add tracking code everywhere—the SDK handles most of it automatically."

## 3. What is OpenTelemetry?
**Diagram Element:** OTel WebSDK diamond component connected to the React frontend.  
**Audio:** "You might wonder: what's OpenTelemetry? Think of it as the 'language' that different monitoring tools speak. The Honeycomb Web SDK uses OpenTelemetry under the hood, which means your traces can connect seamlessly from your React frontend to your backend services—even if they use different programming languages. It's like having a universal translator for your entire application stack."

## 4. Creating Spans for User Actions
**Diagram Element:** Visual representation of a span timeline showing "Go button click" as the root span.  
**Audio:** "When you click 'Go' in Meminator, the SDK creates what's called a 'span'—think of it as a stopwatch that measures how long something takes. This first span represents the user's action and becomes the 'root' of a trace. A trace is like a family tree showing everything that happens because of that one button click."

## 5. HTTP Requests and Context Propagation
**Diagram Element:** HTTP request arrows from React frontend to backend services, labeled with trace IDs and HTTP headers.  
**Audio:** "Here's where it gets interesting. When your React app makes API calls to fetch images or generate memes, the SDK automatically adds special headers to those requests—like a tracking number on a package. This 'trace context' contains a unique ID that connects your frontend click to whatever happens in the backend. It's how we can follow the complete journey of a user's action through your entire system."

## 6. Backend Service Integration
**Diagram Element:** Three distinct backend service boxes (Image, Phrase, Composition) with interconnecting arrows.  
**Audio:** "As Meminator calls different backend services—one for images, one for text, one for composition—that same trace ID flows through each service. This creates a complete picture: you can see not just that a meme generation was slow, but exactly which service caused the delay. However, this only works if your backend services are configured to read and forward those trace headers."

## 7. When Things Go Wrong
**Diagram Element:** Error icon connected to a span showing failed API call with error details.  
**Audio:** "Real-world applications have errors and failures. When something goes wrong in Meminator—like an image service being down or a network timeout—the SDK captures these errors as part of the trace. Instead of just seeing 'meme generation failed,' you can see exactly where and why it failed, making debugging much easier."

## 8. Exporter Configuration
**Diagram Element:** Configuration layer between the OTel WebSDK and Collector, showing API key and endpoint settings.  
**Audio:** "The exporter is like the shipping department for your trace data. It's configured with your Honeycomb API key and knows where to send the telemetry data. The SDK batches this data efficiently, so it doesn't impact your app's performance—users won't notice the monitoring is there."

## 9. Performance Considerations
**Diagram Element:** Performance meter showing minimal overhead, with toggle switches for sampling and filtering.  
**Audio:** "A common concern with frontend monitoring is performance impact. The Honeycomb Web SDK is designed to be lightweight and uses techniques like sampling—only capturing a percentage of traces in high-traffic scenarios—and batching to minimize overhead. Your Meminator app will perform just as well with observability as without it."

## 10. React Router Integration and Page Navigation
**Diagram Element:** Router layer within the React frontend showing route transitions, with spans for navigation events.  
**Audio:** "When users navigate between different pages in Meminator—like going from the home page to the meme gallery—React Router handles these transitions. The SDK automatically creates spans for route changes, so you can track page load times, see which pages users visit most, and identify slow navigation patterns."

## 11. Custom Instrumentation Points
**Diagram Element:** Plus (+) symbols at specific locations in React frontend with dotted lines to custom span boxes.  
**Audio:** "While automatic instrumentation covers the basics, you can add custom spans for business-specific events. In Meminator, you might want to track how long image selection takes, which meme templates are most popular, or user preferences. Adding custom spans is simple—just a few lines of code using the OpenTelemetry API."

## 12. Session and User Context Management
**Diagram Element:** Session Context box above the React frontend, connecting to multiple spans across services.  
**Audio:** "Understanding user behavior requires connecting related activities. You can attach session IDs and user context to spans—like tagging all spans from a single user session. This lets you see patterns: how many memes does a typical user generate? Which features do they use most? This context turns individual traces into insights about user behavior."

## 13. Common Troubleshooting Scenarios
**Diagram Element:** Troubleshooting flowchart showing missing headers, broken traces, and debugging steps.  
**Audio:** "Here are common issues you might encounter: missing trace headers mean your frontend and backend traces don't connect—check that your backend services are reading the 'traceparent' header. Broken traces often indicate network issues or service timeouts. The Honeycomb UI helps you identify these patterns quickly, showing you exactly where traces are breaking."

## 14. Honeycomb Ingest and Visualization
**Diagram Element:** Honeycomb Ingest endpoint connected to a UI mockup showing Meminator trace data.  
**Audio:** "Finally, your trace data arrives at Honeycomb, where it's stored and made available for exploration. In the Honeycomb UI, you can see the complete flow of a meme generation—from button click through all backend services to final composition. You can filter by user sessions, compare performance across different browsers, and identify optimization opportunities. This is where your observability investment pays off: turning raw data into actionable insights."

Note: Here is the diagram we'll use as a baseline, with some additions/changes.
![](./assets/HFO instrumentation setup.png)

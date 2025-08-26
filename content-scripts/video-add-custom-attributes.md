# Video: Adding Custom Attributes to Spans

## Learning Objectives
- Add user and app context to spans in a browser app using OpenTelemetry  
- Choose semantic, consistent attribute names (reuse conventions where possible)  
- Understand how custom attributes make queries and traces more powerful  
- Apply privacy and best practice considerations when enriching spans  

---

**[Audio]**  
Jess, our frontend engineer, has spans flowing from her browser app - Meminator. There's a lot of great context here, but she wants to see the `user.id` and `feature.flag` for each user interaction. 

She can do this by adding custom attributes. Custom attributes let you answer your specific questions about your system performance, like which users were affected during an outage or what feature was active during that interaction.

In this video, you’ll learn:  
- What custom attributes are  
- How to add them with the `HoneycombWebSDK` and OpenTelemetry  
- Best practices for naming and privacy  
- Why they unlock more powerful queries in Honeycomb  

## [Section 1: What Are Custom Attributes?]  

**[Visual]**  
Show a trace in Honeycomb. Zoom into attributes panel on a span: highlight `user.id = some-user-id`, `feature.flag = some-feature-flag-name`. 

[Note for SME: I think we may have to go back to the app code to make `feature.flag` relevant and useful. Or, we think of another story that makes some other example of a custom attribute on a FE span useful and awesome to demo].

**[Audio]**  
Custom attributes are key–value pairs you attach to spans. They enrich your telemetry with business or user context. Instead of just seeing “this API call was slow,” you’ll can use attributes to see *who* experienced it, *when*, and *under what conditions*.  

*(Source: [Honeycomb — Add Custom Instrumentation](https://docs.honeycomb.io/send-data/add-custom-instrumentation/))*  

## [Section 2: How to Add Attributes in a Browser App]  

**[Visual]**   

```js
import { HoneycombWebSDK } from '@honeycombio/opentelemetry-web';
import { getWebAutoInstrumentations } from '@opentelemetry/auto-instrumentations-web';

...

const sdk = new HoneycombWebSDK({
  apiKey: 'your ingest api key',
  serviceName: 'react',
  instrumentations: [getWebAutoInstrumentations({
    '@opentelemetry/instrumentation-xml-http-request': configDefaults,
    '@opentelemetry/instrumentation-fetch': configDefaults,
    '@opentelemetry/instrumentation-document-load': configDefaults,
  })],
  resourceAttributes: { // Data in this object is applied to every trace emitted.
    "user.id": user.id, // Specific to your app.
    "feature.fog": feature.flag, // Specific to your app.
  },
});
sdk.start();
```
*(Source: [Honeycomb — Send Browser Data](https://docs.honeycomb.io/send-data/javascript-browser/honeycomb-distribution/))*  

**[Audio]**
Once Jess has initialized her application with the `HoneycombWebSDK`, she can specify extra attributes through the `resourceAttributes` configuration option. This data will be available on every span her instrumentation emits, which makes it easier to correlate `user.id` and `feature.flag` data to important business information.

## [Section 3: Semantic Conventions and Best Practices]

**[Visual]**
Checklist on screen:
 -Reuse OpenTelemetry semantic conventions
- Use lowercase, dot-separated keys
- Add business or UX context
- Avoid PII and unstable values

**[Audio]**
Follow good practices when when adding custom attributes:
- Reuse OpenTelemetry’s semantic conventions wherever possible. For example, `browser.language` or `browser.platform` are already standardized.
- Use lowercase keys with dot notation for clarity, like `session.id`.
- Add attributes that reflect real-world context — feature flags, user roles, steps in a workflow.- Avoid anything personally identifiable, since  raise privacy concerns.

*(Source: [Honeycomb — Best Practices](https://docs.honeycomb.io/send-data/javascript-browser/honeycomb-distribution/) & [OpenTel - Attributes Spec](https://opentelemetry.io/docs/specs/semconv/registry/attributes/))

## [Section 4: Verify the Config Worked in Honeycomb]

**[Visual]**
Honeycomb trace with new attributes populated.

**[Audio]**
Now, let's verify that her custom attributes are actually being sent to Honeycomb.

Jess opens Meminator and clicks `GO` a few times to generate fresh traces. Let's go over to Honeycomb and view one of our most recent traces. Select Fields, then search for `user.id` and `feature.flag`. Ah! They're here. 

## [Section 5: Query with Custom Attributes in Honeycomb]

**[Visual]**
In Query Builder:
- VISUALIZE: COUNT
- GROUP BY: user.id
- ORDER BY: COUNT DESC

**[Audio]**
Now let's use the new custom attributes to query our data. Let's say Jess wants to figure out who the most active users in Meminator are. First, navigate to the Query Buider. 

Add COUNT to the VISUALIZE clause, then GROUP BY `user.id`. ORDER BY `COUNT DESC` and LIMIT to 5.

This will show you the top 5 most active users by number of events/traces.

## [Section 6: Query with Custom Attributes in Honeycomb]

**[Visual]**
In Query Builder:
- VISUALIZE: HEATMAP(duration_ms) 
- GROUP BY: feature.flag

**[Audio]**
What if Jess wants to compare performance between users with different feature flags? She can do that now. In the Query Builder, add `HEATMAP(duration_ms)` to the VISUALIZE clause and GROUP BY `feature.flag`.

This shows you if users with different feature flag settings experience different performance.

## [Section 7: Recap]

**[Visual]**
Slide with takeaways.

**[Audio]**
Let's recap.
- Custom attributes add useful business context like user IDs and feature flags to your spans.
- Implementation is simple - just add them to `resourceAttributes` in your `HoneycombWebSDK` config and they'll appear on every span automatically.
- Follow best practices: use lowercase dot-notation, reuse OpenTelemetry conventions when possible, and avoid personally identifiable information.
- Always validate - check that your attributes show up in Honeycomb's Fields search before relying on them.
- They unlock powerful queries: compare performance across feature flags, find your most active users, and debug issues for specific customers.


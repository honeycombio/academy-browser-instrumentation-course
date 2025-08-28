# Video: Adding Custom Resource Attributes to Spans

## Learning Objectives
- Add user and app context to spans in a browser app using OpenTelemetry  
- Choose semantic, consistent attribute names (reuse conventions where possible)  
- Understand how custom attributes make queries and traces more powerful  
- Apply privacy and best practice considerations when enriching spans  

---

**[Audio]**  
Jess, our frontend engineer, has spans flowing from her browser app: Meminator. She's running an A/B test on a new meme generation flow. Jess realizes she needs deeper visibility: she wants to understand how the new feature is performing AND investigate individual user experiences. She needs to see the `user.id` and `feature.flag` for each user interaction. 

She can do this by adding custom resource attributes. Resource attributes let you answer your specific questions about your system performance, like which users were affected during an outage or what feature was active during that interaction.

In this video, you’ll learn:  
- What custom resource attributes are  
- How to add them with the `HoneycombWebSDK` and OpenTelemetry  
- Best practices for naming and privacy  
- Why they unlock more powerful queries in Honeycomb  

## [Section 1: What Are Custom Attributes?]  

**[Visual]**  
Show a trace in Honeycomb. Zoom into attributes panel on a span: highlight `user.id = some-user-id`, `feature.flag = some-feature-flag-name`. 

[Note for SME & CEd: We have to modify the app code to make `feature.flag` relevant and useful for seeing how well a new feature is performing. Chatted with Ken about this on 8/27; changes to the app code will be made by adding an API and JSON file; file setting would be changed, app restarts.]

**[Audio]**  
Custom resource attributes are key–value pairs you attach to spans. They enrich your telemetry with business or user context. Instead of just seeing “this API call was slow,” you’ll can use attributes to see *who* experienced it, *when*, and *under what conditions*.  

*(Source: [Honeycomb — Add Custom Instrumentation](https://docs.honeycomb.io/send-data/add-custom-instrumentation/))*  

## [Section 2: How to Add Attributes in a Browser App]  

**[Visual]** Code w/ `resourceAttributes` config highlighted

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
    "feature.flag": feature.flag, // Specific to your app.
  },
});
sdk.start();
```
*(Source: [Honeycomb — Send Browser Data](https://docs.honeycomb.io/send-data/javascript-browser/honeycomb-distribution/))*  

**[Audio]**
During initialization of her application with the `HoneycombWebSDK`, she can specify extra attributes through the `resourceAttributes` configuration option. This data will be available on every span her instrumentation emits, which makes it easier to correlate `user.id` and `feature.flag` data to important business information.

## [Section 3: Semantic Conventions and Best Practices]

**[Visual]**
Checklist on screen:
- Reuse OpenTelemetry semantic conventions
- Consider using named constants instead of hard-coded strings
- Use lowercase, dot-separated keys
- Add business or UX context
- Avoid PII and unstable values

**[Audio]**
When adding custom attributes, there are some good practices you should follow: 
- Reuse OpenTelemetry’s semantic conventions wherever possible. For example, `browser.language` or `browser.platform` are already standardized. 
- Consider using named constants instead of hard-coded strings. 
- Use lowercase keys with dot notation for clarity, like `session.id`.
- Add attributes that reflect real-world context — feature flags, user roles, steps in a workflow.
- Avoid anything personally identifiable, since these can raise privacy concerns.

*(Source: [Honeycomb — Best Practices](https://docs.honeycomb.io/send-data/javascript-browser/honeycomb-distribution/) & [OpenTel - Attributes Spec](https://opentelemetry.io/docs/specs/semconv/registry/attributes/)) [Add for Named Constants for the SDK: Link for SDK https://github.com/open-telemetry/opentelemetry-js/tree/main/semantic-conventions]

## [Section 4: Verify the Config Worked in Honeycomb]

**[Visual]**
Rerun Meminator, then open Web Launchpad. GROUP BY `feature.flag`. Then FILTER BY `user.id`.

**[Audio]**
Now, let's verify that her custom resource attributes are actually being sent to Honeycomb.

Jess reruns Meminator with the new config and clicks `GO` a few times to generate fresh traces.

Jess is looking at the Web Launchpad in Honeycomb. Her first question: How is each variant of her A/B test performing? She starts by grouping her data by `feature.flag` to see overall system performance split between her control group and expirimental groups. Great! She sees the `feature.flag` attribute in the GROUP BY dropdown. Now, she can see a high-level view of how her new feature is impacting performance across all users.

Now Jess wants to get more granular. She filters down to a specific `user.id` to understand their experience in detail. Looks like `user.id` is here! She can now see exactly how a specific person's interaction with her application looks. 

If she notices something interesting in the data, she can investigate further by clicking into a specific trace. In the Query Builder, she can use COUNT to see all of the events a specific user generated during an interaction.

**[Visual]**
Slide with takeaways.

**[Audio]**
Let's recap.
- Custom resource attributes add useful business context like user IDs and feature flags to your spans.
- Implementation is simple - just add them to `resourceAttributes` in your `HoneycombWebSDK` config and they'll appear on every span automatically.
- Follow best practices: use lowercase dot-notation, reuse OpenTelemetry conventions when possible, and avoid personally identifiable information.
- Always validate - check that your attributes show up in Honeycomb's Fields search before relying on them.
- They unlock powerful queries: compare performance across feature flags, find your most active users, and debug issues for specific customers.

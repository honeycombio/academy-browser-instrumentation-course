# Video: Adding Custom Resource Attributes to Spans

## Learning Objectives
- Add app context to spans in a browser app using OpenTelemetry  
- Choose semantic, consistent attribute names  
- Understand how custom attributes make queries and traces more powerful  
- Apply privacy and best practice considerations when enriching spans  

---

**[Audio]**  
Jess, our frontend engineer, has spans flowing from her browser app: Meminator. She's running an A/B test on a new meme generation flow. Jess realizes she needs deeper visibility: she wants to understand how the new feature is performing. She needs to see the feature flag for each user interaction. 

She can do this by adding custom resource attributes. Resource attributes let you answer your specific questions about your system performance, like which users were affected during an outage or what feature was active during that interaction.

## [Section 1: What Are Custom Attributes?]  

**[Visual]**  
Show a full-stack trace in Honeycomb. Zoom into attributes panel on a span: highlight `app.feature.flag.user.questions` field.

**[Audio]**  
Custom resource attributes are key–value pairs you attach to spans. They enrich your telemetry with business or user context. Instead of just seeing “this API call was slow,” you can use attributes to see *who* experienced it, *when*, and *under what conditions*.  

*(Source: [Honeycomb — Add Custom Instrumentation](https://docs.honeycomb.io/send-data/add-custom-instrumentation/))*  

## [Section 2: How to Add Attributes in a Browser App]  

**[Visual]** Code w/ `resourceAttributes` config highlighted

```js
import { HoneycombWebSDK } from '@honeycombio/opentelemetry-web';
import { getWebAutoInstrumentations } from '@opentelemetry/auto-instrumentations-web';
import { features } from './features';
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
    "app.feature.flag.user.questions": features.ALLOW_USER_QUESTIONS, // Specific to your app.
  },
});
sdk.start();
```
*(Source: [Honeycomb — Send Browser Data](https://docs.honeycomb.io/send-data/javascript-browser/honeycomb-distribution/))*  

**[Audio]**
During initialization of her application with the `HoneycombWebSDK`, Jess can specify extra attributes through the `resourceAttributes` configuration option. This data will be available on every span her instrumentation emits, which makes it easier to correlate feature flag data to important business information. 

In our example, we only add one resource attribute; however, in this hash, you can add as many resource attributes as you would like to.

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
Show the code snippet `features.ts` that's setting the feature flag to `false`. then open Web Launchpad. GROUP BY `app.feature.flag.user.questions`. Then FILTER BY `user.id`.

**[Audio]**
Now, let's verify that her custom resource attributes are actually being sent to Honeycomb. In the code, the `ALLOW_USER_QUESTIONS` feature flag is in `features.ts` in the source directory. It's currently set to `false` by default.

Jess runs Meminator with the new config and clicks `Generate Meme!` a few times to generate fresh traces. This is one workflow of Meminator: users can press `Generate Meme` and it will pair an image with a stock phrase.

Then, Jess sets the feature flag to `true`, restarts the application with `./run` and reloads Meminator in her browser. Now she sees the option to type in her own phrase, then presses `GO` to generate fresh traces. This is the second meme generation workflow: users can type in their own phrase, which will be paired with an image.

Now, Jess is looking at the Web Launchpad in Honeycomb. Her first question: How is each variant of her A/B test performing? She starts by grouping her data by `app.feature.flag.user.questions` to see overall system performance split between her control group and expirimental group. Great! She sees the feature flag attribute she instructed in the GROUP BY dropdown. Now, she can see a high-level view of how her new meme generation flow is impacting performance across all users.

If she notices something interesting in the data, she can investigate further by clicking into a specific trace. 

**[Visual]**
Slide with takeaways.

**[Audio]**
Let's recap.
- Custom resource attributes add useful business context to your spans.
- Implementation is simple - just add them to `resourceAttributes` in your `HoneycombWebSDK` config and they'll appear on every span automatically.
- Follow best practices: use lowercase dot-notation, reuse OpenTelemetry conventions when possible, and avoid personally identifiable information.
- Always validate - check that your attributes show up in Honeycomb's Fields search before relying on them.
- They unlock powerful queries: compare performance across feature flags, find your most active users, and debug issues for specific customers.

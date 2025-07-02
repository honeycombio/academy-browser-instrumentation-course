# Video: Why Frontend Observability

## Learning Objectives
- Describe why frontend telemetry completes the observability picture
- Identify problems developers can solve using Honeycomb frontend observability (HFO)

---

**[Visual]** Title screen: Bold text on neutral background: "Why Frontend Observability Matters"  
**[Audio]** "Meet Jess — a frontend engineer who's just pushed a new signup flow to production."

**[Visual]** Jess at a dual-monitor setup. One screen shows a real analytics dashboard (conversion funnel view), the other shows a code editor. A notification pops up.  
**[Audio]** "At first, everything looks fine. The page loads, the button appears. But minutes later, user signups plummet — and support tickets start rolling in."

**[Visual]** Jess opens a team messaging app. Snippets from customer support appear: 'Signup page keeps freezing.' 'I click the button and nothing happens.'  
**[Audio]** "Reports say the signup page is freezing, or worse, the signup button does nothing."

**[Visual]** Jess opens Chrome DevTools. Network tab shows a handful of API calls, all green. Lighthouse audit reads 'Performance: 92'. A Core Web Vitals panel is visible.  
**[Audio]** "Jess checks the frontend tools she trusts: no obvious errors, good performance scores. So... why are users stuck?"

**[Visual]** Cut to a side-by-side view: backend trace data on the left, logs on the right. Traces show a fast response time from `POST /signup`.  
**[Audio]** "The backend shows clean logs and successful traces. From the moment the request hits the server, everything runs smoothly."

**[Visual]** Jess leans back, visibly frustrated. She rubs her temples, then returns to the keyboard.  
**[Audio]** "But Jess knows something’s off — and she has no visibility into what’s really happening before that request reaches the server."

**[Visual]** Montage: A third-party script loading slowly, an unresponsive click handler, a missed navigation state change — all with no trace context.  
**[Audio]** "Is a third-party script delaying the click event? Did a single-page app route change break state? Without full visibility, she’s flying blind."

**[Visual]** Honeycomb UI showing a trace with no frontend spans — only backend service names and durations.  
**[Audio]** "Traces help — but only from the backend forward. There’s nothing showing what happened in the browser, before the API call."

**[Visual]** Jess installs the Honeycomb Web SDK. Rebuilds and refreshes her app. Now the trace loads in full: spans for click, document load, and XHR.  
**[Audio]** "Then Jess adds frontend tracing. Now the full picture loads."

**[Visual]** Honeycomb UI trace view: spans labeled `click`, `documentLoad`, and `fetch`, ordered top-down with durations, attributes panel open.  
**[Audio]** "She sees the trace begin with a user click, tagged with the page URL and user agent. Then a navigation timing span shows a delay in document load. Finally, an XHR span captures the API call — all stitched together."

**[Visual]** Jess opens the span attributes panel. Keys like `session.id`, `user.role`, and `feature.flag` are clearly labeled with values.  
**[Audio]** "Jess drills into the click span and sees useful context — session ID, user role, even the feature flag the user was bucketed into."

**[Visual]** Jess clicks the `documentLoad` span and expands a child span labeled `analytics.js`. Duration: 1.3s.  
**[Audio]** "She opens the document load span and spots a 1.3-second delay — caused by a third-party analytics script blocking the render."

**[Visual]** Code editor open. Jess comments out a script include, pushes a fix. Trace reloads in Honeycomb: no delay.  
**[Audio]** "She isolates the delay, removes the blocking script, and confirms the improvement in Honeycomb."

**[Visual]** Text overlay on light background: "Problems solved with fullstack tracing:" followed by animated bullet points:  
- Page loads are slow — pinpoint the frontend bottleneck  
- Users drop off — identify where interaction broke  
- Errors spike — trace them across the stack  
**[Audio]** "With frontend observability, you see what users do, what the browser does, and how it all connects to the backend."

**[Visual]** Jess glances at her trace, leans back with a smile. A teammate behind her nods, impressed.  
**[Audio]** "Jess can finally build with confidence — knowing that if something breaks, she’ll see exactly where and why."

**[Visual]** Fullscreen text: "Frontend observability completes your traces"  
**[Audio]** "Because fullstack observability isn’t just for backend engineers — it’s for everyone who wants to understand user experience."

**[Visual]** Final screen: "Next: Learn how to trace the browser." with Honeycomb logo bottom corner.  
**[Audio]** "Let’s dive in and explore how to instrument your frontend — and unlock the other half of your traces."

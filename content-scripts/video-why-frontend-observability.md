# Video: Why Frontend Observability

## Learning Objectives
- Describe why frontend telemetry completes the observability picture
- Identify problems developers can solve using Honeycomb frontend observability (HFO)

---

(Note: A lot of the specific examples (e.g. names of features or spans) are from ChatGPT — I asked it to make my examples more specific and real, but was made without app code to actually refer to... it needs to be revised to be accurate to what Meminator's actually doing, but the gist is there!)

**[Visual]** Title screen: Bold text on neutral background: "Why Frontend Observability Matters"  
**[Audio]** "Meet Jo — a frontend engineer who's just pushed a new meme creation flow to production."

**[Visual]** Jo at a dual-monitor setup. One screen shows a real analytics dashboard (conversion funnel view), the other shows a code editor. A notification pops up.  
**[Audio]** "At first, everything looks fine. The page loads, the button appears. But minutes later, meme creations plummet — and support tickets start rolling in."

**[Visual]** Jo opens a team messaging app. Snippets from customer support appear: 'Meming page keeps freezing.' 'I click the button and nothing happens.'  
**[Audio]** "Reports say the meming page is freezing, or worse, the "go" button does nothing."

**[Visual]** Jo opens DevTools. Network tab shows a handful of API calls, all green. Lighthouse audit reads 'Performance: 92'. A Core Web Vitals panel is visible.  
**[Audio]** "Jo checks the frontend tools she trusts: no obvious errors, good performance scores. So... why are users stuck?"

**[Visual]** Cut to a side-by-side view: backend trace data on the left, logs on the right. Traces show a fast response time from [name of the relevant service as it's written in the app].  
**[Audio]** "The backend shows clean logs and successful traces. From the moment the request hits the server, everything runs smoothly."

**[Visual]** Jo leans back, visibly frustrated. She rubs her temples, then returns to the keyboard.  
**[Audio]** "But Jo knows something’s off — and she has no visibility into what’s really happening before that request reaches the server."

**[Visual]** Montage: A third-party script loading slowly, an unresponsive click handler, a missed navigation state change — all with no trace context.  
**[Audio]** "Is a third-party script delaying the click event? Did a single-page app route change break state? Without full visibility, she’s flying blind."

**[Visual]** Honeycomb UI showing a trace with no frontend spans — only backend service names and durations.  
**[Audio]** "Traces help, but we can only see what's happening in the backend. There’s nothing showing what happened in the browser..."

**[Visual]** Jo installs the Honeycomb Web SDK. Rebuilds and refreshes her app. Now the trace loads in full: spans for click, document load, and XHR.  
**[Audio]** "Then Jo adds frontend tracing. Now the full picture emerges."

**[Visual]** Honeycomb UI trace view: spans labeled according to the app, ordered top-down with durations, attributes panel open.  
**[Audio]** "She sees the trace begin with a user click, tagged with the page URL and user agent. Then a navigation timing span shows a delay in document load. Finally, an XHR span captures the API call — all stitched together!" 

**[Visual]** Jo opens the span attributes panel. Keys like `session.id`, `user.role`, and `feature.flag` are clearly labeled with values.  
**[Audio]** "Jo drills into the click span and sees useful context — session ID, user role, even the feature flag the user was bucketed into."

**[Visual]** Jo clicks the `documentLoad` span and expands a child span labeled `analytics.js`. Duration: []  
**[Audio]** "She opens the document load span and spots a delay — caused by a third-party analytics script blocking the render."

**[Visual]** Code editor open. Jo comments out a script include, pushes a fix. Trace reloads in Honeycomb: no delay.  
**[Audio]** "She isolates the delay, removes the blocking script, and confirms the improvement in Honeycomb."

**[Visual]** Text overlay on light background: "Problems solved with fullstack tracing:" followed by animated bullet points:  
- Page loads are slow — pinpoint the frontend bottleneck  
- Users drop off — identify where interaction broke  
- Errors spike — trace them across the stack  
**[Audio]** "With frontend observability, you see what users do, what the browser does, and how it all connects to the backend."

**[Visual]** Jo glances at her trace, leans back with a smile. A teammate behind her nods, impressed.  
**[Audio]** "Jo can finally build with confidence, knowing that if something breaks, she’ll see exactly where and why."

**[Visual]** Fullscreen text: "Frontend observability completes your traces"  
**[Audio]** "Because fullstack observability isn’t just for backend engineers; it’s for everyone who wants to understand user experience."

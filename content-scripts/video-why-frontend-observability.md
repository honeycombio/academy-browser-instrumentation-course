# Video: Why Frontend Observability

## Learning Objectives
- Describe why frontend telemetry completes the observability picture
- Identify problems developers can solve using Honeycomb frontend observability (HFO)

---

(Note: A lot of the specific examples (e.g. names of features or spans) are from ChatGPT — I asked it to make my examples more specific and real, but was made without app code to actually refer to... it needs to be revised to be accurate to what Meminator's actually doing, but the gist is there!)

**[Visual]** Title screen: Jo, FE engineer, and the write-your-own-phrase feature of Meminator on screen.
**[Audio]** "Meet Jo — a frontend engineer who's just pushed a new meme creation flow to production."

**[Visual]** Jo at a dual-monitor setup. One screen shows a real analytics dashboard (performance by URLs view or similar), the other shows a code editor and browser with Meminator open. A notification pops up.  
**[Audio]** "At first, everything looks fine. The page loads, users can type in a phrase, the 'GO' button appears. But minutes later, meme creations plummet — and support tickets start rolling in."

**[Visual]** Jo opens her team messaging app. Snippets from customer support appear: 'Meming page keeps freezing.' 'I type in my phrase, click the button, and nothing happens.'  
**[Audio]** "Reports say the meming page is freezing, or worse, the button does nothing."

**[Visual]** Jo opens DevTools. Network tab shows a handful of API calls, all green. Lighthouse audit reads 'Performance: 92'. A Core Web Vitals panel is visible.  
**[Audio]** "Jo checks the frontend tools she trusts: no obvious errors, good performance scores. It works when she tests it. So... why are users stuck?"

**[Visual]** Cut to Honeycomb to view backend trace data. Traces show a fast response time from `backend-for-frontend`.  
**[Audio]** "The backend shows successful traces. From the moment the request hits the server, everything runs smoothly."

**[Visual]** Jo leans back, visibly frustrated. She rubs her temples, then returns to the keyboard. 
**[Audio]** "But Jo knows something’s off and she has no visibility once the request leaves the browser."

**[Visual]** Montage: From RUM tool: A third-party script loading slowly, an unresponsive click handler — all with no trace context.  
**[Audio]** "Is a third-party script delaying the click event? Did a single-page app route change break state? Without full visibility, she’s flying blind."

**[Visual]** Honeycomb UI showing a trace with no frontend spans — only backend service names and durations. 
**[Audio]** "Traces help, but we can only see what's happening in the backend. There’s nothing showing what happened in the browser..."

**[Visual]** Jo installs the Honeycomb Web SDK. Rebuilds and refreshes her app. Now the trace loads in full: spans for click, document load, and fetch.  
**[Audio]** "Then Jo adds frontend tracing in Honeycomb. Now the full picture emerges. We can use Honeycomb to debug our application in production because we, as developers, can't always anticipate how a user will break our application."

**[Visual]** Honeycomb UI full-stack trace view: service names and span names properly labeled, ordered top-down with durations, attributes panel open. [Note On-Screen: Sometimes the click is its own trace. What you're most interested in is the `HTTP POST`.] 
**[Audio]** "Now that we've instrumented the frontend, we see traces with errors. Here's an example. She sees the trace begin with a user click, containing the user agent and other useful attributes. Oh! It looks like there is an error marked on some of the the spans in this dataset... " 

**[Visual]** Jo reviews spans in the failed trace, and clicks on "spans with errors" button, and she can go to the last span with errors by clicking the up arrow from the first span. 
**[Audio]** "She explores the attributes of this span by searching for the word 'exception.' Aha! Now she sees that the user fed in a phrase that contained an exclaimation point, which caused the service to fail. Now she knows the backend should be fixed - it needs to sanitize its input before attempting to render it."

**[Visual]** Text overlay on light background: "Problems solved with fullstack tracing:" followed by animated bullet points:  
- Slow page loads — find the exact bottleneck
- Slow API calls - identify which service calls are failing and why
- Errors that break user flow - catch exceptions across your entire stack before users do
**[Audio]** "With frontend observability, Jo can see what her users are doing, how the frontend application behaves, and how it all connects to the backend in a single trace."

**[Visual]** Jo smiles.
**[Audio]** "She can finally build with confidence, knowing that if something breaks, she’ll see exactly where and why."

**[Visual]** Fullscreen text: "Frontend observability completes your traces"  
**[Audio]** "Because fullstack observability isn’t just for backend engineers; it’s for everyone who wants to understand all service interactions that make up the full user experience."

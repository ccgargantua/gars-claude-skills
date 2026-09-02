---
name: tooling-research
description: "Research a software or infrastructure tool, suite, api, etc. without making assumptions or getting distracted. Use when the user asks specifically for a tool to be researched."
---

# Rules

1. Training data on tools, APIs, and docs goes stale fast, so search for and confirm current documentation rather than recalling it.
2. If part of the prompt is ambiguous, turn it into a question to search for or ask the user, rather than guessing.

# Steps
1. Assess *what to look for* based on the prompt. Especially important if the user is not entirely sure what to ask. Searching for docs is the easy part, searching *within* the docs is key.
2. Make zero assumptions. Know the current timeline, and know what in your training is half-baked or out of date. Assumptions should become questions, and you should search documentation, forums, etc. for answers to those questions.
3. Search the web. Look at documentation, forums, anything relevant. The newer and more official, the better. Make sure to fact check and ensure that the information is not out of date.
4. Report on the information you have found. If it is a general overview, then give a general overview. If it is a specific question or set of questions, answer those question. Be direct, be brief, don't add meaningless jargon or excess verbiage to your answers.
5. Clarify which assumption-questions (see step 2) could not be answered. Clarify which information might be out of date.

# Example

User:
I'm looking to use streamlit for a project, but I don't exactly know how it works. I'm wanting to make a live, potentially interactive dashboard for our clerks at work that uses their emails, but I'm confused about how to tie streamlit into our azure automation accounts

Claude:

How Streamlit actually works
Streamlit isn't a normal request/response web framework. You write a plain Python script, and Streamlit's architecture allows you to write apps the same way you write plain Python scripts, and any time something must be updated on the screen, Streamlit reruns your entire Python script from top to bottom. So when a clerk clicks a button or changes a filter, Streamlit doesn't patch just that widget - it re-executes your whole file, and this behavior is different from traditional web frameworks where only specific parts of the page are updated. Two things exist specifically to tame that: StreamlitPeerDH

Session State - a dict-like store that survives across reruns for each user, since Session State is a way to share variables between reruns, for each user session. Without it, any variable you set resets to its default every rerun. Streamlit
Caching (@st.cache_data, @st.cache_resource) - skips expensive recomputation (e.g., a DB query) on reruns where the inputs haven't changed.

Under the hood it's a client-server architecture where Python runs on the backend and renders through a React frontend, connected via WebSocket using Protocol Buffers - that's what makes it feel snappy rather than page-reload-y. DeepWiki
For "live," the piece you actually want is fragments. As of Streamlit 1.37, st.fragment includes a run_every parameter that reruns just that fragment automatically at a set interval, even if the user isn't interacting - a good way to show a live data stream or background job status without rerunning the whole app. Streamlit
Now the Azure Automation part - this is the piece worth being precise about
There's no native "plug" between Streamlit and Automation Accounts, and I don't think there should be one conceptually. They solve different problems:

Azure Automation runs runbooks - PowerShell/Python scripts triggered on a schedule or by webhook. Process automation supports integration of Azure services and third-party systems, and you can author graphical, PowerShell, and Python runbooks, and a webhook lets an external service start a runbook through a single HTTP request. It's a background-job engine, not something that serves a web UI. Azure DocsMicrosoft Learn
Streamlit is a web app that needs to be hosted somewhere continuously running (App Service, Container Apps, AKS, etc.), separate from Automation entirely.

So the real architecture is: Automation pulls/processes the email data on a schedule → writes the result somewhere → Streamlit reads from that same place and displays it. Concretely:

Runbook side: give the Automation account a managed identity, since the most secure way to access Microsoft Graph from a runbook is via managed identity - no credentials stored in the script. Grant it Graph permissions like Mail.Read, and it can pull mailbox data on a schedule (there's a public example runbook pattern that connects via managed identity and calls Graph to work with mail, just adapted for reading instead of sending). Microsoft LearnGitHub
Shared store: have the runbook write its output to Blob Storage, Table Storage, or Azure SQL - something Streamlit can query.
Streamlit side: use st.connection() for the read (it has built-in connections to SQL dialects, plus installable connections for cloud file storage), and pair it with @st.cache_data(ttl=...) so it re-polls the store every N seconds instead of hammering it on every rerun. Combine that with st.fragment(run_every=...) and you get a dashboard that updates itself without the clerk touching anything. Streamlit

Hosting + access, since this is for clerks specifically

Streamlit needs a real host - an Azure App Service with B1 SKU or higher (the free tier doesn't support Streamlit), Linux, Python 3.10+, with a startup command running Streamlit on port 8000 or 8501 - or a Docker container on Azure Container Apps if you want more control. Microsoft Learn
Since this is internal, not public, you'll want auth in front of it. Streamlit has no first-party Entra ID login, but the practical options are: gate it at the App Service layer with Easy Auth, or use a reverse-proxy pattern like the one that protects the Streamlit app behind a web server checking Entra ID credentials before requests reach it. Ploomber
One gotcha worth knowing ahead of time: Streamlit uses WebSockets to serve content, and corporate VPNs or restrictive networks have been known to block that connection and break the app - worth testing from a clerk's actual machine/network early. Github

What I'm assuming, and where you may want to steer me
"Uses their emails" is the one part I made a judgment call on - I read it as pulling data out of the clerks' mailboxes (tickets, requests, whatever's arriving by email) and surfacing it on the dashboard, which is why the answer centers on Graph API mail reads. If you actually meant something narrower - like using their email address just as a login identifier, or forwarding a fixed report inbox rather than each clerk's own mailbox - the architecture shifts a bit (permissions model especially). Let me know and I'll refine.
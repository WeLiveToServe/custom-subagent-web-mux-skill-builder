# Custom Subagent Web Mux Skill Builder

Build a small, inexpensive discovery agent that learns how a person actually uses the web.

It turns recurring work into dependable, low-latency runbooks.

The intended output is not a general-purpose browsing agent. It is a growing library of narrowly scoped skills: concise instructions that tell an execution agent the best available way to complete a familiar task on a particular service.

## The problem

General-purpose agents often rediscover the same workflow every time. They may browse a UI, read large bodies of documentation, inspect APIs, or drive a browser by screenshots before doing a simple repeatable task. That wastes time and context, and creates more chances for a brittle workflow to fail.

For a recurring task, the best path may be much simpler. For example, searching an email service with a precise query and opening one attachment is generally better than scanning a mailbox and interpreting every possible match.

Different websites expose different access points: APIs, CLIs, MCP servers, and browser-native interfaces. Some expose none that are useful for the task, leaving browser automation or computer use as the only practical option. The emerging WebMCP standard is especially promising because it could expose user-facing browser actions in a compact, agent-friendly form.

The discovery agent should proactively identify each site’s available access points and smoke-test authorized candidates. It should decide both the best general way to access a site and, when several options exist, the best path for a common repeated user task or request.

Those access points shift over time. A UI changes, an API is revised or its documentation drifts, a CLI command evolves, an MCP tool’s behavior changes, or a browser-native integration gains or loses capability. A previously strong workflow can therefore become fragile without warning.

This project aims to make the judgment once, record it clearly, and verify it continuously.

## Goal

Given a website and a user activity, determine and maintain the execution path that best balances:

1. Reliability
2. Low latency
3. Low context/token use
4. Minimal and appropriate access

The system should be able to perform most research, evaluation, and routine validation with a modern low-cost model, such as OpenAI Luna, Gemini Flash, or Claude Haiku. More capable models should be reserved for ambiguous, high-impact, or failed cases.

## Access paths to evaluate

For each site and task, the discovery agent inventories the available interfaces and chooses the smallest suitable one.

| Path | Best use | Typical tradeoff |
| --- | --- | --- |
| API | Stable, structured operations with available authorization | Requires credentials and knowledge of a service-specific schema |
| CLI | Scriptable operations supported by a vendor tool | Often broad and verbose; command discovery can add context cost |
| MCP | Structured tools exposed by a service | Quality and coverage vary by provider |
| Browser-native/WebMCP-style interfaces | User-facing actions in the user’s signed-in browser | Emerging ecosystem and browser support may vary |
| Browser automation / computer use | A fallback when no better interface exists | Slowest and most fragile; sensitive to UI, layout, and state changes |

An API or CLI is not automatically the right answer. The selected path should serve the actual user task with the least complexity, rather than exposing an agent to every capability a provider offers.

## How it should work

1. **Discover recurring work.** With explicit user consent, review selected browsing history, user-nominated sites, and optionally private local observations to identify repeat activities.
2. **Define the task.** Convert an observed behavior into a bounded, human-readable goal—for example, “find Amanda’s bidet attachment” rather than “search email.”
3. **Inventory interfaces.** Identify documented APIs, CLIs, MCP servers, browser-exposed capabilities, and feasible UI automation paths.
4. **Evaluate candidate workflows.** Test only authorized, safe paths and score them for success rate, latency, token/context cost, permissions required, and maintenance burden.
5. **Create a runbook.** Store the winning workflow as a compact skill with inputs, preconditions, exact tool sequence, expected outputs, recovery guidance, and a validation check.
6. **Monitor and improve.** An inexpensive maintenance agent revisits the preferred paths daily, tests the runbooks, and checks for changes in workflows, APIs and their documentation, MCP tools, and browser-native/WebMCP capabilities. It proposes the smallest update needed to restore reliability, latency, and context efficiency, and escalates only when a workflow changes materially or needs user action.

## Runbook principles

- Prefer deterministic interfaces before UI automation.
- Prefer targeted queries and narrow tools over exhaustive searching or broad documentation ingestion.
- Keep each skill task-specific and small enough to load only when needed.
- Reuse the user’s existing authenticated browser state where an appropriate browser-native mechanism permits it; do not casually export cookies or bypass authentication controls.
- Treat credentials, financial information, health information, and other sensitive data as explicit-scope access only.
- Preserve a safe fallback path and say when the system needs human approval, authentication, or judgment.

## Initial scope

Start with a handful of user-nominated web tasks that occur often and have clear success criteria. Build the smallest viable pipeline:

- a task/site inventory;
- a repeatable interface audit;
- a scoring rubric for competing execution paths;
- a structured runbook format;
- a validator that checks whether a stored runbook still works; and
- a review queue for proposed changes.

Browser-history mining, passive observation, automatic daily checks, and broad cross-site discovery are later phases. They should be opt-in, transparent, and designed around local-first data handling.

## Success looks like

When an execution agent receives a familiar request, it should load one short, current runbook and complete the work through the appropriate interface—without re-learning the website, re-reading extensive documentation, or crawling the UI unless that is genuinely necessary.

## Original notes

The unedited stream-of-consciousness source for this README is retained in [`keith-scope-dump`](./keith-scope-dump).

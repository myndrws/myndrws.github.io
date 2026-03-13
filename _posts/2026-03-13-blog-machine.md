---
title: "Blog Machine: Using GitHub MCP and Claude Code to write about work"
date: 2026-03-13
---

There's a particular kind of procrastination that feels almost productive: I’ve done interesting work, I want to write about it, and instead I spend forty minutes staring at a blank document wondering how to start. Recently I tried to short-circuit this with a small experiment - using Claude Code, wired up to GitHub via the MCP, to help me turn completed tickets and pull requests into blog posts. I used this process for my most recent post ahead of this one on [open sourcing the Innovation Clusters map](https://myndrws.github.io/2026/03/12/open-sourcing-innoclusters.html) - this was my test case.

Here's how I evolved the process into something I want to keep using.

## Getting started

The [GitHub MCP](https://github.com/github/github-mcp-server#local-github-mcp-server) server is an official tool from GitHub that exposes the GitHub API as a set of callable tools for AI agents. Rather than copying and pasting issue descriptions and PR diffs into a chat window, I configured it to run locally via Docker, giving Claude Code read-only access to my organisation's repos. The relevant toolsets (issues, pull requests, repos, projects) get spun up automatically when Claude needs them and tear themselves down afterwards. 

The configuration lives in a CLAUDE.md file in a dedicated working directory. Claude Code reads this at the start of every session, so the instructions, constraints, and output preferences are always in play without me having to repeat them. I saved the setup in a bash script so that I could recreate the MCP if I needed, and that was about it.
Iteration

My first instinct was to let Claude do it autonomously: point it at a ticket, have it read the issue thread, the PR diff, the review comments, and produce a draft. And it did - competently, quickly, and weirdly making me sound super catty:

------
> "_Some reflections on the surprisingly valuable bureaucracy of making code public_.
>
> _When we decided to open source the Innovation Clusters frontend, I expected a straightforward task: flip the repository to public, add a license file, done. What I didn't expect was how much the process itself would expose about our assumptions [...] about what it actually means to hand code over to strangers on the internet._"

_Claude’s incorrect interpretation of my feelings on open sourcing._

-----

To clarify, I did not expect the open-sourcing process to be straightforward, but I also didn’t experience the assumption-shattering enlightenment, or the ‘bureaucracy’, suggested by this paragraph, either. The problem isn't that it was technically wrong – it was all in the framing, and it wasn’t particularly suited for a public audience. I had prompted it to use my voice as based on previous posts and the tickets I’d written – and, apparently, I have a persistent sarcastic undertone that I need to address. 

The second approach introduced an interview phase. After gathering all the GitHub context, Claude presents its research findings in a few bullet points, then asks up to five specific questions before writing anything. These are about why a particular technical decision was made, what we considered and rejected, what the outcome actually felt like in practice. This also gives me a chance to steer the model away from things that it thinks are super important (e.g., *shock horror, I discovered you didn’t solve all the low-priority reliability findings from a code scan, I must write about this!*). 

---

> _Phase 2: Interview Questions_
>
> _1. On the decision to open source: The issue mentions [...] - was there internal debate about whether to open source, or wa sit always the plan once the app was published?_
>
> [...]
>
> _4. On the dummy data approach: Creating dummy data so the app runs without AWS authentication efels like a key design decision for usability. Was there discussion about alternative approaches?_ 

_Some of the interview questions Claude asked me to develop the post_ 

---

The difference was immediate. Having to answer those questions even briefly forced me to articulate things that weren't in the ticket. The resulting draft was closer to how I'd describe the work to a colleague.  

Even with the interview process, something was still slightly off. There are turns of phrase, shallow semantic depth and instances of the ever-telling em dash that give away Claude’s personality (that of a [teacher's pet](https://www.theguardian.com/technology/2026/feb/03/gemini-grok-chatgpt-claude-qwen-ai-chatbots-identity-crisis)  with all the homogeneity you’d expect from something trained on internet-writing patterns) rather than mine. Further, the posts were more personal but still a little too assured. My actual writing tends to sit with uncertainty more, ask questions, use parenthetical asides, (I don’t love all these things but they’re currently true). Hard things to specify in advance.

To mitigate this, after editing the Claude-drafted post into something I was happy with, I asked Claude to examine the diff between its draft and my edited version, and to add new instructions to the CLAUDE.md based on what it found. The output was a set of concrete additions to the tone section. These were specific enough to be actionable, and grounded in actual evidence rather than my vague intuitions about my own style. It included stuff like:

---

>- _Use past tense narrative ("When the map became live") not present tense_
>- _Avoid defensive framing ("wasn't an afterthought", "wasn't just pragmatic")_
>- _Use softer transitions ("This said" rather than "But")_

_A few of the guidelines Claude wrote for itself to sound more like me._

---

This is, I think, the more interesting part of the experiment. The CLAUDE.md file is now functioning as an updatable memory - a record of what works, refined over time by comparing output to edited output. Each post I write and edit is a data point. The blog machine should, in theory, get incrementally better at sounding like me without me having to explicitly teach it every nuance upfront. 

The finished product for the test case, [open sourcing the Innovation Clusters map, is published here](https://myndrws.github.io/2026/03/12/open-sourcing-innoclusters.html).

## Using the blog machine

This isn't about removing myself from the writing. The interview phase matters precisely because it requires me to think. The editing matters because that's where my actual voice shows up. What the system removes is the activation energy problem - the blank page, the "where do I even start" that sends me back to my inbox.

The GitHub MCP means the raw material is already assembled before I've typed a word. The interview means I'm articulating things I'd have fumbled towards anyway, just faster. And the active learning loop means the process improves without me having to be deliberate about it. Lastly, this process is helping me to become more aware of my own writing style and how to improve it, which I hope will serve as an antidote to producing writing that tends toward the Claude-mean – I now have to go and prove it.

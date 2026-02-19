---
title: "Guarding against reverse centaurs"
date: 2026-02-19
---

### Some reflections on balancing the use of AI with the use of my brain.

*This is based entirely my own personal experience and does not represent the views of my employer.*

I have been thinking a lot about what Cory Doctorow refers to as the "reverse centaur". To quote a [recent Guardian article](https://www.theguardian.com/us-news/ng-interactive/2026/jan/18/tech-ai-bubble-burst-reverse-centaur) directly, "A reverse centaur is a machine head on a human body, a person who is serving as a squishy meat appendage for an uncaring machine". Doctorow is building a bigger argument - but I kept thinking about whether or not this was the road I, personally, was sliding down - or worse, if it was a road I could be contributing to building.

# In development 

Like a lot of developers, I've been doing a lot of my work using various forms of copilot coding. When I use these tools cleverly and thoughtfully, I at least feel like I'm doing more work (though I haven't measured any time savings myself, and [I don't know that I would find them](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)). Some examples of ‘cleverly and thoughtfully’ include: iteratively designing configuration; creating mermaid-based documentation to support a codebase; requesting critiques for obvious errors I've made when defining my testing suite. 

Sometimes, perhaps less cleverly and thoughtfully, I let the agent I’m working with have free reign. Both successes and failures often result in additional complexity – redundant code, unused config, convolutions and dead-ends that would pile up if unchecked. Even if the code works, it's mostly not working in the most elegant, explainable way. It’s my responsibility to weed out the good from the bad in what the copilot has done. But as copilots become ever more competent, and leave me in the dust for what makes good code, my task for spotting that good job versus a bad one becomes harder. I’m aware that, like the problem for driverless vehicles, the issue is that the [mistakes become so rare as for me to lose the ability to spot them](https://www.ft.com/content/6734cdc4-5e2d-403b-8f3b-467c1d4eebfc). Unlike driverless vehicles, we’re talking about a matter of months (weeks?) for automation complacency to set in, not years. My job becomes only to take the blame for when the AI has made a real error - as Doctorow points out, this is the real risk to workers in general.

Currently, this amounts to a personal risk of enfeeblement - losing my hard-won skills because I lose practice, thereby becoming completely reliant on AI. And that feels risky because AI is not infallible. So, at least for now, I enforce some real cognitive barriers to ensure critical thought engagement. These are additional steps in my workflow such that I have to truly engage with a problem - for instance, planning what a pipeline might look like from the perspective of the person who has to run it. I maintain control of the problem solving and the system design, so that I am doing the interesting work, but also so that I can fully explain these aspects to other developers, stakeholders and users. For example, this looks like diagramming out the system myself first to get my head around the problem, before allowing agents to improve and execute the implementation, and using narrower task-setting (i.e., “critique this solution” rather than “what’s the answer here”). Often, I don't allow the copilot agentic access to write to the codebase, because using the conversational feature alone forces me to think about exactly what has been done, and make adjustments. In this way, I stolidly enforce myself as the agentic human-in-the-loop.

# In design

Wrestling with my own practice led me to a more worrying question - am I propagating the reverse-centaur to users of things I create? For instance, if I build an application which makes use of AI for efficiency gains, does my work lead other people to become the risk-sinks in the loop – the person who gets the blame when the machine doesn't do the job and otherwise retains no real agency?

I have worked on projects where AI enables a task that would otherwise take users a lot of time. But designs and implementations are imperfect, and AI is unreliable – so the mitigation strategy is often to make it clear that the person using the tool retains responsibility for checking the output, for accepting the risk that the tool hasn't done the job well.

This is absolutely a standard industry pattern – every time you use any chatbot, this is what you're essentially signing up to. You gain information, but take on the risk of assuming that information is correct, fact-checking to a point where you’re comfortable with the risk. That doesn’t make it the correct pattern to follow for every problem. 

I think that one of the key questions to ask in designing these kinds of products is whether or not the task can be “[Goodharted](https://en.wikipedia.org/wiki/Goodhart%27s_law)” -  will the measure being used likely become a target, and therefore cease to be a good measure? For example, if the measure is “number of documents assessed per hour” and the target is to hit as many documents as possible in that hour, does the tool assign true agency to the user or to the machine - will the person be pressured to keep up with AI-level workloads, inevitably cutting risk corners to achieve this? 

Take for example a situation in which an AI tool is extracting some messy documents, because the documents are received from lots of sources in loads of different formats and with no consistency. We could build an AI-based tool that extracts just the numbers required - but it cannot deterministically and with 100% accuracy get those numbers. So to solve that problem, a sensible-feeling solution is that a human should check the output. So far so good - the human retains agency, the machine helps them process - problem solved.

Now in deployment, this tool is used at scale - now a task that would have taken a human reviewer days is done in minutes. In theory, this saves the humans time - in practice and at scale, they just need to process more documents. To review the number of documents that the AI can process, the human must also review many more documents. They are still subject to manual error, and are now a risk-sink for clearing the volumes of AI numbers that they could otherwise never be fully checked. 

By considering the counterfactual, i.e. how would this problem be solved if AI was not an available tool, alongside the questions above, we keep the solution space wider for longer. In the document-processing example, maybe a better solution would have just been to change the system, changing the data collection format to be consistent and deterministically machine-readable, tackling the problem at the root. 

I also think that this issue can be somewhat resolved with more thoughtful design for the evaluating and monitoring needed. If the situation is a slightly different one, in which only high accuracy and not total accuracy is acceptable, there is more opportunity to genuinely improve the use of the human reviewer’s time by “featurising” evaluation - for instance, by training a model to detect instances that the AI tool might struggle with most and flagging only these as requiring review. In this way, useful skills to add to the developer toolbox include an appreciation for the nuance of the strengths and weaknesses of their tool and the human-in-the-loop as a system, and the ability to implement and then action the insights from the evaluation of these.

# Going forward

My practices are evolving alongside the AI toolkit (particularly in regard to Model Context Protocol, which I’m currently upskilling in), and I’m sure they reflect my personal biases to how I want to solve problems and develop my career. I find hearing other people’s reflections on thoughtful use of AI tools really helpful (for instance, [Robin Linacre’s thoughts)](https://www.robinlinacre.com/respectful_use_of_ai/). For now, being consciously aware of how and why I’m using and designing AI tools hopefully goes some way to keeping me (and the people using what I build) as the centaur's head on a machine body, rather than the reverse. It's what I'm using to guide becoming a more thoughtful practitioner, rather than a risk-sink for automation.

*With thanks to Rachel Dunne for reviewing this post!*


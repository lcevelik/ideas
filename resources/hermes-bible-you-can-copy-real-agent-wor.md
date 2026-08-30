Source: https://youtu.be/t_GxN2Gwqsk?si=jGr3fhkyGmQgpuJS
Title: Hermes Bible: You Can Copy REAL Agent Workflows
Duration: 10:53 (652.5s)
Transcript source: captions
============================================================

[00:01] Okay, folks, we've been preaching since day one that the best way to work with
[00:03] day one that the best way to work with your Hermes agent is to feed them the
[00:05] your Hermes agent is to feed them the official documentation or even the LLM
[00:08] official documentation or even the LLM text version here. Feed it to your
[00:10] text version here. Feed it to your Hermes agent and save this as a
[00:12] Hermes agent and save this as a knowledge base or a database or even
[00:15] knowledge base or a database or even in relevant context files. So, they boot
[00:17] in relevant context files. So, they boot this up in every session. So, that way
[00:20] this up in every session. So, that way whatever workflow you want to set up
[00:22] whatever workflow you want to set up with your Hermes agent, they can always
[00:24] with your Hermes agent, they can always refer to the official docs and set it up
[00:26] refer to the official docs and set it up according to best practices. But, there
[00:29] according to best practices. But, there is a problem here. There's some some
[00:31] is a problem here. There's some some limitations here with the official docs.
[00:33] limitations here with the official docs. Is it's very fragmented and often times
[00:37] Is it's very fragmented and often times it lacks real
[00:39] it lacks real workflow setups, real workflow examples
[00:42] workflow setups, real workflow examples that you can draw inspiration from or
[00:44] that you can draw inspiration from or even copy. You can check on next, you
[00:46] even copy. You can check on next, you can check on Discord or or any other
[00:48] can check on Discord or or any other social media platforms, but it is very
[00:51] social media platforms, but it is very fragmented. So, what the community did
[00:53] fragmented. So, what the community did was they built the Hermes Bible. In
[00:56] was they built the Hermes Bible. In fact, we came across this yesterday.
[00:58] fact, we came across this yesterday. This was built by Luke the Dev, but
[01:00] This was built by Luke the Dev, but anyone can submit their workflows here
[01:03] anyone can submit their workflows here so that other people can copy or draw
[01:06] so that other people can copy or draw inspirations from. So, it's not
[01:07] inspirations from. So, it's not affiliated with the Nous research team,
[01:10] affiliated with the Nous research team, but it indexes their entire knowledge
[01:13] but it indexes their entire knowledge base and adds the production layer on
[01:15] base and adds the production layer on top. So, stuff that people are actually
[01:17] top. So, stuff that people are actually running today, you can
[01:19] running today, you can draw your reference here in the Hermes
[01:21] draw your reference here in the Hermes Bible. So, today in this video, we're
[01:23] Bible. So, today in this video, we're going to walk through the Hermes Bible.
[01:26] going to walk through the Hermes Bible. We're going to talk about some of the
[01:27] We're going to talk about some of the sections that I find very helpful and
[01:29] sections that I find very helpful and I'll show you exactly what to bookmark
[01:32] I'll show you exactly what to bookmark for your everyday work. Okay, so
[01:33] for your everyday work. Okay, so starting off with the hidden features,
[01:35] starting off with the hidden features, this is like a cheat sheet for power
[01:37] this is like a cheat sheet for power users. These are very useful commands
[01:40] users. These are very useful commands and tips and tricks to use for your
[01:42] and tips and tricks to use for your Hermes agent workflow
[01:44] Hermes agent workflow from day to day. But, what I noticed is
[01:46] from day to day. But, what I noticed is they lack the they don't have the tmux
[01:49] they lack the they don't have the tmux trick. So, the tmux basically keeps your
[01:51] trick. So, the tmux basically keeps your session on 24/7 until you exit. So, even
[01:54] session on 24/7 until you exit. So, even if you're on VPS, if there's like if
[01:56] if you're on VPS, if there's like if your internet got disconnected or maybe
[01:58] your internet got disconnected or maybe something uh uh got interrupted, your
[02:01] something uh uh got interrupted, your task will completely stop until
[02:04] task will completely stop until uh your you know, until you interact
[02:06] uh your you know, until you interact with your Hermes agent. So, what you can
[02:08] with your Hermes agent. So, what you can do is you can run tmux
[02:11] do is you can run tmux and then you CD into a particular
[02:12] and then you CD into a particular project. So, we have a project uh
[02:15] project. So, we have a project uh directory called AI research, run
[02:17] directory called AI research, run Hermes.
[02:19] Hermes. And when you see this colored bar here,
[02:21] And when you see this colored bar here, it means your tmux session is on. So,
[02:25] it means your tmux session is on. So, basically, even if I close this, right?
[02:28] basically, even if I close this, right? I go back
[02:30] I go back and type tmux a. So, tmux a is just the
[02:32] and type tmux a. So, tmux a is just the recent tmux session,
[02:34] recent tmux session, I can go back here. So, I can close this
[02:37] I can go back here. So, I can close this window even mid task or mid prompt and
[02:40] window even mid task or mid prompt and nothing ever gets interrupted. And what
[02:43] nothing ever gets interrupted. And what I can show you as well as if you have
[02:44] I can show you as well as if you have multiple uh tmux sessions, so I'll show
[02:47] multiple uh tmux sessions, so I'll show you here.
[02:54] You can type tmux ls and you'll see the tmux sessions that are currently active.
[02:57] tmux sessions that are currently active. So, the this one, the three, this is the
[02:59] So, the this one, the three, this is the Hermes one that I showed you just now.
[03:01] Hermes one that I showed you just now. This zero one is a Claude code session
[03:03] This zero one is a Claude code session that I have been running. So, what you
[03:05] that I have been running. So, what you can do is go tmux attach -t zero, so
[03:10] can do is go tmux attach -t zero, so whatever the number here is, attach and
[03:12] whatever the number here is, attach and it'll take you back to the uh tmux
[03:15] it'll take you back to the uh tmux session. So, this is very useful if
[03:17] session. So, this is very useful if you're running loops, if you have an
[03:19] you're running loops, if you have an agent running a task every 10 minutes or
[03:22] agent running a task every 10 minutes or every 30 minutes and you don't really
[03:24] every 30 minutes and you don't really need human in the loop, you can use the
[03:27] need human in the loop, you can use the tmux and then just run /goal and give it
[03:30] tmux and then just run /goal and give it a task for your Hermes agent and keep
[03:32] a task for your Hermes agent and keep running it. Another page that can be
[03:34] running it. Another page that can be very useful is the Hermes desktop. If
[03:36] very useful is the Hermes desktop. If you're using the app, uh it's a full
[03:39] you're using the app, uh it's a full tour of the desktop app. It contains
[03:41] tour of the desktop app. It contains every tips and tricks that you need to
[03:42] every tips and tricks that you need to know for running the Hermes uh desktop
[03:45] know for running the Hermes uh desktop app. It also includes the remote back
[03:48] app. It also includes the remote back end connecting if you're running a VPS
[03:50] end connecting if you're running a VPS because, you know, Hermes desktop app is
[03:52] because, you know, Hermes desktop app is local. If you have your Hermes agent on
[03:54] local. If you have your Hermes agent on VPS, you need to connect it to a remote
[03:56] VPS, you need to connect it to a remote backend, which we in fact did a video
[03:59] backend, which we in fact did a video guide on how to do that. You can check
[04:01] guide on how to do that. You can check that one out. Uh, but any, you know,
[04:03] that one out. Uh, but any, you know, troubleshooting and useful tips you can
[04:04] troubleshooting and useful tips you can find it here. Now, the next one you'll
[04:06] find it here. Now, the next one you'll get a lot of value from. This is the 15
[04:09] get a lot of value from. This is the 15 levels of Hermes agent usage. You can
[04:11] levels of Hermes agent usage. You can find it under the flows page here. And
[04:14] find it under the flows page here. And this is sort of like a cheat sheet as
[04:15] this is sort of like a cheat sheet as well. It's like a complete roadmap of
[04:18] well. It's like a complete roadmap of your mastery of Hermes agent. So, level
[04:21] your mastery of Hermes agent. So, level zero, level one, these are your one-shot
[04:24] zero, level one, these are your one-shot prompts, uh, basic chat with memory.
[04:27] prompts, uh, basic chat with memory. Level two uh, introduces the apprentice
[04:29] Level two uh, introduces the apprentice model. So, memory plus soul.md. Level
[04:33] model. So, memory plus soul.md. Level three is commands and skills. Level four
[04:35] three is commands and skills. Level four adds integration. Level five is
[04:38] adds integration. Level five is orchestration with parallel sub-agents
[04:41] orchestration with parallel sub-agents using MCPs. Level six is the builder
[04:44] using MCPs. Level six is the builder tier. So, scheduled and async tasks.
[04:47] tier. So, scheduled and async tasks. This is the delegate tasks uh, update
[04:49] This is the delegate tasks uh, update that we talked about this morning, the
[04:51] that we talked about this morning, the v0.17 update. Uh, level seven is the
[04:55] v0.17 update. Uh, level seven is the agentic power users. So, much more
[04:57] agentic power users. So, much more higher levels of async opera- uh,
[05:00] higher levels of async opera- uh, operations using slash goal. So, it's
[05:02] operations using slash goal. So, it's also another loops engineering use case
[05:04] also another loops engineering use case that you can make use of. And it goes
[05:07] that you can make use of. And it goes all the way up to level 14. And I find a
[05:10] all the way up to level 14. And I find a lot of value from this, because you can
[05:12] lot of value from this, because you can read this flow and know exactly where
[05:14] read this flow and know exactly where you are stuck. Most people are actually
[05:17] you are stuck. Most people are actually stuck at level zero or level one,
[05:19] stuck at level zero or level one, because they never configure their
[05:21] because they never configure their contacts file properly. I wouldn't put
[05:23] contacts file properly. I wouldn't put too much emphasis on the soul.md file,
[05:26] too much emphasis on the soul.md file, but much more so for the agents.md
[05:30] but much more so for the agents.md and the user.md. Now, the slash goal is
[05:33] and the user.md. Now, the slash goal is one of the loop engineering use cases
[05:35] one of the loop engineering use cases that you can use in Hermes agent. This
[05:38] that you can use in Hermes agent. This gets its own full guide, and you should
[05:41] gets its own full guide, and you should be using this more often. This is the
[05:42] be using this more often. This is the command that turns a chat into a
[05:44] command that turns a chat into a structured task. The flow here covers
[05:47] structured task. The flow here covers every sub command, how to write
[05:48] every sub command, how to write measurable goals, and the recommended
[05:51] measurable goals, and the recommended workflows. There's also a companion flow
[05:53] workflows. There's also a companion flow with 21 copy-paste commands across six
[05:56] with 21 copy-paste commands across six categories. So, there's for research,
[05:59] categories. So, there's for research, lead generation, content, email,
[06:01] lead generation, content, email, operations, and development as well. So,
[06:04] operations, and development as well. So, if you're starting off with {slash}
[06:05] if you're starting off with {slash} goal, I recommend research first. This
[06:07] goal, I recommend research first. This is the easiest that you can do. But,
[06:09] is the easiest that you can do. But, before you go ahead and start a new
[06:10] before you go ahead and start a new session, make sure you start a new
[06:13] session, make sure you start a new project directory, okay? So, it's very
[06:15] project directory, okay? So, it's very similar to the ones I've shown you. Uh
[06:18] similar to the ones I've shown you. Uh since day one, okay? Just make a new
[06:20] since day one, okay? Just make a new folder in your VPS or on local machine,
[06:23] folder in your VPS or on local machine, and then CD into that project. You can
[06:25] and then CD into that project. You can even have tmux on, pair that with
[06:27] even have tmux on, pair that with {slash} goal, and you get yourself
[06:29] {slash} goal, and you get yourself already a loop system that you can run.
[06:31] already a loop system that you can run. So,
[06:32] So, uh you can try {slash} goal uh instead
[06:35] uh you can try {slash} goal uh instead of research five competitors, you can
[06:37] of research five competitors, you can say uh research Claude code or whatever,
[06:41] say uh research Claude code or whatever, but give it a time as well. Let's say
[06:43] but give it a time as well. Let's say every 5 minutes, loop this one, and then
[06:46] every 5 minutes, loop this one, and then what give it a file to output as well as
[06:49] what give it a file to output as well as the markdowns. And uh if you're using
[06:51] the markdowns. And uh if you're using LLM Wiki as well,
[06:53] LLM Wiki as well, and then you'll get a full output every
[06:56] and then you'll get a full output every 10 minute, 30 minute, but don't uh
[06:59] 10 minute, 30 minute, but don't uh expect the output would be 100% correct.
[07:01] expect the output would be 100% correct. So, the thing about the loop engineering
[07:04] So, the thing about the loop engineering we find is that because it's turning out
[07:06] we find is that because it's turning out so much work, it sort of lacks the
[07:08] so much work, it sort of lacks the adversarial review loop or like, you
[07:11] adversarial review loop or like, you know, QC, fact-checking, all that.
[07:14] know, QC, fact-checking, all that. You'll need another agent to
[07:17] You'll need another agent to verify that the work is in fact
[07:19] verify that the work is in fact fact-checked or quality-checked. So,
[07:21] fact-checked or quality-checked. So, that's one tip I would add here into the
[07:24] that's one tip I would add here into the Hermes Bible. But, out of these
[07:25] Hermes Bible. But, out of these examples, I wouldn't use uh code
[07:27] examples, I wouldn't use uh code refactoring for the case. Hermes agent,
[07:29] refactoring for the case. Hermes agent, just think of it as an extension of
[07:31] just think of it as an extension of yourself. They're really good for uh
[07:33] yourself. They're really good for uh easy mundane tasks that takes a lot of
[07:35] easy mundane tasks that takes a lot of time if you were to do it yourself. But,
[07:37] time if you were to do it yourself. But, for actual code ones, you want to run
[07:39] for actual code ones, you want to run them on code uh coding harnesses like
[07:42] them on code uh coding harnesses like Claude code or Kimmy code, Kilo code,
[07:45] Claude code or Kimmy code, Kilo code, all of that. In fact, you can even set
[07:47] all of that. In fact, you can even set up a slash goal to have your Hermes
[07:49] up a slash goal to have your Hermes agent as an orchestrator to use Claude
[07:52] agent as an orchestrator to use Claude code or Kimmy code or whatever code to
[07:55] code or Kimmy code or whatever code to run those
[07:56] run those uh coding projects. Now, if you're an
[07:58] uh coding projects. Now, if you're an advanced engineer, okay, this is where
[08:00] advanced engineer, okay, this is where you're at pro level. It's also useful
[08:03] you're at pro level. It's also useful for beginners to get a reference of,
[08:04] for beginners to get a reference of, okay, what do the actual power users
[08:07] okay, what do the actual power users look like? So, this is the Jira
[08:09] look like? So, this is the Jira pipeline. So, it's an event-driven
[08:11] pipeline. So, it's an event-driven engineering intake agent watches for
[08:14] engineering intake agent watches for tickets, then the coding agents writes
[08:16] tickets, then the coding agents writes the implementation, review agent audits
[08:19] the implementation, review agent audits the code, the CI agent runs the
[08:21] the code, the CI agent runs the pipeline, and you as a human would keep
[08:24] pipeline, and you as a human would keep the merge authority for these PRs. And
[08:26] the merge authority for these PRs. And the architecture detail here uh that
[08:28] the architecture detail here uh that matters is Jira serves as the source of
[08:31] matters is Jira serves as the source of truth for team visible work, while
[08:33] truth for team visible work, while Hermes internal Kanban handles
[08:35] Hermes internal Kanban handles agent-to-agent coordination. So, the
[08:37] agent-to-agent coordination. So, the agents use the Kanban board to claim
[08:39] agents use the Kanban board to claim tasks, block on dependencies, and
[08:41] tasks, block on dependencies, and unblock each other. This flow includes
[08:43] unblock each other. This flow includes the exact tool configuration, uh the
[08:46] the exact tool configuration, uh the model selection as well for each agent
[08:48] model selection as well for each agent here, and the cost estimate, which is
[08:50] here, and the cost estimate, which is about, I think, $12 here each for each.
[08:53] about, I think, $12 here each for each. Now, if you're doing a lot of research
[08:54] Now, if you're doing a lot of research on X and you don't want to pay with API
[08:56] on X and you don't want to pay with API key, but you are paying for X Premium,
[08:59] key, but you are paying for X Premium, you can copy this workflow where you uh
[09:01] you can copy this workflow where you uh give Hermes agent Grok to search
[09:05] give Hermes agent Grok to search trending news on X, and it shows you the
[09:07] trending news on X, and it shows you the stack that you you can go for to run
[09:11] stack that you you can go for to run these workflows. So, it's very, very
[09:12] these workflows. So, it's very, very useful, very helpful if you rely a lot
[09:15] useful, very helpful if you rely a lot of your research on X. Now, more on to
[09:17] of your research on X. Now, more on to loops, there's a community member that
[09:19] loops, there's a community member that recommends eight loops that you can run
[09:21] recommends eight loops that you can run inside Hermes agent that can also
[09:23] inside Hermes agent that can also compound their results over time. You
[09:26] compound their results over time. You can see that the foundation of most of
[09:27] can see that the foundation of most of these loops use slash goal for those
[09:31] these loops use slash goal for those tasks. There's also self-improvement
[09:33] tasks. There's also self-improvement loop, curator loop, memory loop, Kanban
[09:37] loop, curator loop, memory loop, Kanban dispatcher loop, compression loop, uh
[09:40] dispatcher loop, compression loop, uh sub-agent loop for your Hermes agent.
[09:43] sub-agent loop for your Hermes agent. So, uh what you can do is you can in
[09:45] So, uh what you can do is you can in fact feed this to your Hermes agent, ask
[09:48] fact feed this to your Hermes agent, ask them, you know, what loops can I run for
[09:50] them, you know, what loops can I run for the task? Uh but remember, you don't
[09:52] the task? Uh but remember, you don't need to have every task running loops
[09:54] need to have every task running loops because uh that might be very redundant,
[09:57] because uh that might be very redundant, but mostly for system checks, that's
[09:59] but mostly for system checks, that's very useful. Or if you want a research
[10:01] very useful. Or if you want a research note every 30 minutes or every hour,
[10:03] note every 30 minutes or every hour, that's also another loop use case that
[10:05] that's also another loop use case that you can run, but you need, you know,
[10:07] you can run, but you need, you know, that fact-checking, verifying uh
[10:09] that fact-checking, verifying uh component as well for running those
[10:12] component as well for running those loops. But it's mostly for uh code-heavy
[10:14] loops. But it's mostly for uh code-heavy work that you'd need loops. Okay? So, uh
[10:18] work that you'd need loops. Okay? So, uh you can bookmark any of these flows and
[10:21] you can bookmark any of these flows and refer them uh and come back to them
[10:23] refer them uh and come back to them every time you need help for those
[10:26] every time you need help for those workflows that you're running. So, if
[10:27] workflows that you're running. So, if you have a workflow that is working in
[10:29] you have a workflow that is working in production, you can submit it here as
[10:31] production, you can submit it here as well and grow the knowledge base of the
[10:33] well and grow the knowledge base of the Hermes Bible. A lot of the power users
[10:35] Hermes Bible. A lot of the power users are in fact looking at this, submitting
[10:37] are in fact looking at this, submitting the flow. Uh so, I expect maybe in the
[10:40] the flow. Uh so, I expect maybe in the next few weeks we're we're going to see
[10:41] next few weeks we're we're going to see an even more workflow examples that we
[10:44] an even more workflow examples that we can copy. All right? So, if you find
[10:46] can copy. All right? So, if you find this video helpful, smash up the like
[10:47] this video helpful, smash up the like button, subscribe to the channel, follow
[10:49] button, subscribe to the channel, follow for more updates and guides. My name is
[10:50] for more updates and guides. My name is Ron. Signing out.

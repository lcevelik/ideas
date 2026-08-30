Source: https://youtu.be/gYzlgK5Dw1s?is=BbNpnj-T8W1DI-17
Title: Claude Code + Hermes Agent = $10,000 AI Agents
Duration: 19:14 (1153.6s)
Transcript source: captions
============================================================

[00:02] Businesses are paying $10,000 for AI agents that realistically now only take
[00:05] agents that realistically now only take an afternoon to build. Thanks to Hermes,
[00:07] an afternoon to build. Thanks to Hermes, which is by far one of the most powerful
[00:09] which is by far one of the most powerful agentic platforms in the world right
[00:12] agentic platforms in the world right now. And it becomes even stronger when
[00:13] now. And it becomes even stronger when you pair it up with Claude code, which
[00:16] you pair it up with Claude code, which is why in this video I'm going to be
[00:17] is why in this video I'm going to be breaking down this exact tool [music]
[00:19] breaking down this exact tool [music] stack and build one of these agents live
[00:22] stack and build one of these agents live right in front of you start to finish on
[00:24] right in front of you start to finish on this screen. [music] And the best part
[00:26] this screen. [music] And the best part is that I'm not going to use any code,
[00:28] is that I'm not going to use any code, I'm not going to have any team behind
[00:29] I'm not going to have any team behind me, and I'm not going to skip through
[00:31] me, and I'm not going to skip through any of the boring but important parts of
[00:33] any of the boring but important parts of the actual build. And at the end of the
[00:35] the actual build. And at the end of the video, I'm also going to go through the
[00:37] video, I'm also going to go through the part which most people struggle with,
[00:39] part which most people struggle with, which is how you can actually sell these
[00:41] which is how you can actually sell these systems for thousands of dollars to
[00:43] systems for thousands of dollars to businesses. [music]
[00:44] businesses. [music] So, if it sounds like something that you
[00:45] So, if it sounds like something that you want to learn, let's dive in. All right,
[00:47] want to learn, let's dive in. All right, so before we get to the actual build, I
[00:50] so before we get to the actual build, I want to show you that we've done this,
[00:51] want to show you that we've done this, meaning that we've sold AI agents to
[00:53] meaning that we've sold AI agents to dozens of different companies, one of
[00:55] dozens of different companies, one of which was a service company and we
[00:56] which was a service company and we closed the deal for $18,000 across four
[00:59] closed the deal for $18,000 across four different months. On the screen right
[01:01] different months. On the screen right now, you should see a signed contract as
[01:03] now, you should see a signed contract as proof that we've done this and hopefully
[01:04] proof that we've done this and hopefully you can do it too. So, with that said,
[01:06] you can do it too. So, with that said, let's get into the build. Okay, so we're
[01:08] let's get into the build. Okay, so we're going to use Claude code and Hermes
[01:09] going to use Claude code and Hermes agent to build a $10,000 AI agent, which
[01:12] agent to build a $10,000 AI agent, which in this case is Speed to Lead. Now,
[01:14] in this case is Speed to Lead. Now, there was a study that came out that
[01:16] there was a study that came out that reported that the average business owner
[01:17] reported that the average business owner takes 47 hours to respond to a lead and
[01:21] takes 47 hours to respond to a lead and responding in 5 minutes instead of 30
[01:23] responding in 5 minutes instead of 30 makes you about 100 times more likely to
[01:25] makes you about 100 times more likely to reach them and 21 times more likely to
[01:27] reach them and 21 times more likely to qualify them. Meaning that every single
[01:29] qualify them. Meaning that every single lead that they don't follow up with is
[01:31] lead that they don't follow up with is lost revenue for the business. Hence why
[01:33] lost revenue for the business. Hence why you are able to charge $10,000 plus for
[01:35] you are able to charge $10,000 plus for this exact system right here. So, the
[01:37] this exact system right here. So, the way that it works is that we have a
[01:38] way that it works is that we have a form, in this case it can be a Facebook
[01:40] form, in this case it can be a Facebook ads, or Google ads, or just a simple
[01:42] ads, or Google ads, or just a simple form on the website. The lead will fill
[01:44] form on the website. The lead will fill out the form and then we have an AI
[01:45] out the form and then we have an AI agent that calls them within 10 seconds
[01:48] agent that calls them within 10 seconds of them opting in. It will ask them some
[01:50] of them opting in. It will ask them some questions, it will book a property
[01:52] questions, it will book a property inspection because it's a roofing
[01:54] inspection because it's a roofing company. So, my roof is broken, it's
[01:56] company. So, my roof is broken, it's leaking, X Y and Z. So, it will ask
[01:57] leaking, X Y and Z. So, it will ask questions about that, and it will book
[01:59] questions about that, and it will book the the actual inspection, the free
[02:01] the the actual inspection, the free inspection for the for the lead itself.
[02:04] inspection for the for the lead itself. And then we add details to a database,
[02:06] And then we add details to a database, which will be a Google Sheet. Now, the
[02:08] which will be a Google Sheet. Now, the first step is actually uh setting up the
[02:10] first step is actually uh setting up the hosting platforms for both different uh
[02:12] hosting platforms for both different uh softwares. So, we have VS Code for Clock
[02:14] softwares. So, we have VS Code for Clock Code, and we have Hostinger for the
[02:16] Code, and we have Hostinger for the Hermes Agent. To download VS Code, you
[02:18] Hermes Agent. To download VS Code, you can go to visualstudio.com/download,
[02:21] can go to visualstudio.com/download, download it for your desktop, and then
[02:22] download it for your desktop, and then you'll be able to get on a page that
[02:24] you'll be able to get on a page that looks like this. You can go to
[02:25] looks like this. You can go to extensions, and then you can download
[02:26] extensions, and then you can download the Clock extension right here. Press
[02:28] the Clock extension right here. Press install, and you'll see that you have it
[02:30] install, and you'll see that you have it right here. And then we want to add or
[02:31] right here. And then we want to add or open a new folder in our desktop in this
[02:33] open a new folder in our desktop in this case, um and we can name this Hermes
[02:37] case, um and we can name this Hermes Agent.
[02:40] Agent. There we go.
[02:41] There we go. Open.
[02:42] Open. Yes. Open this. And we're all set up for
[02:44] Yes. Open this. And we're all set up for the Clock Code that we're using as an
[02:45] the Clock Code that we're using as an extension inside of VS Code. Now, in
[02:48] extension inside of VS Code. Now, in terms of Hostinger, you can go to
[02:49] terms of Hostinger, you can go to hostinger.com/applications/hermesagent,
[02:51] hostinger.com/applications/hermesagent, and you'll get here. I recommend that
[02:53] and you'll get here. I recommend that you get the KVM2 plan option just
[02:55] you get the KVM2 plan option just because it gives you the most
[02:56] because it gives you the most flexibility with the power, but also the
[02:58] flexibility with the power, but also the memory itself. And once you log in, you
[03:00] memory itself. And once you log in, you get the plan, you'll get on a page that
[03:02] get the plan, you'll get on a page that looks like this, where you have your
[03:03] looks like this, where you have your Docker um installed. And you can go to
[03:05] Docker um installed. And you can go to manage, and then you can go to Docker
[03:07] manage, and then you can go to Docker Manager. You can install this, it will
[03:09] Manager. You can install this, it will take about a minute. And once you have
[03:10] take about a minute. And once you have this here, you can go to compose.
[03:12] this here, you can go to compose. You can go to one-click deploy. Look for
[03:15] You can go to one-click deploy. Look for Hermes Agent inside of the catalog here.
[03:18] Hermes Agent inside of the catalog here. So, you can do Hermes
[03:20] So, you can do Hermes Agent select. So, make sure to copy this
[03:22] Agent select. So, make sure to copy this and save this somewhere safe. You can
[03:24] and save this somewhere safe. You can press deploy, and now it will take about
[03:25] press deploy, and now it will take about 30 seconds to 1 minute to actually
[03:27] 30 seconds to 1 minute to actually deploy in this server itself. All right,
[03:29] deploy in this server itself. All right, now the Hermes Agent is opened, it's
[03:31] now the Hermes Agent is opened, it's finished. We can go here to open, and it
[03:33] finished. We can go here to open, and it will ask us for a username and password,
[03:35] will ask us for a username and password, which is the one that we had before. So,
[03:37] which is the one that we had before. So, we can paste Hermes here,
[03:39] we can paste Hermes here, and we can also paste the password that
[03:41] and we can also paste the password that we had, as well.
[03:44] we had, as well. And now we're inside the actual system.
[03:46] And now we're inside the actual system. As you can see from the beautiful
[03:47] As you can see from the beautiful interface, this is Hermes uh dashboard,
[03:49] interface, this is Hermes uh dashboard, is where you chat to Hermes. You can say
[03:50] is where you chat to Hermes. You can say hello.
[03:51] hello. And it says error because we have not
[03:53] And it says error because we have not connected the AI model yet, which is
[03:54] connected the AI model yet, which is fine. But, this is what you should see.
[03:56] fine. But, this is what you should see. If you don't see this, something went
[03:58] If you don't see this, something went wrong. Go back into me. All right, so
[03:59] wrong. Go back into me. All right, so that's it in terms of the hosting for
[04:00] that's it in terms of the hosting for Hermes agent and Claude code. Now, we
[04:02] Hermes agent and Claude code. Now, we get to the actual softwares that we need
[04:04] get to the actual softwares that we need to use. So, the first one here is
[04:05] to use. So, the first one here is Anthropic for the API key. So, you can
[04:07] Anthropic for the API key. So, you can go to platform.cloud.com, setting
[04:10] go to platform.cloud.com, setting workspaces default keys. You can press
[04:12] workspaces default keys. You can press create key, and you can name this Hermes
[04:15] create key, and you can name this Hermes agent or whatever it is that you want to
[04:16] agent or whatever it is that you want to name it. You can press copy key,
[04:19] name it. You can press copy key, and you find them actually you have
[04:20] and you find them actually you have enough credits here. I currently have
[04:22] enough credits here. I currently have $17 because you're going to need this to
[04:24] $17 because you're going to need this to run the agent itself. Then we have
[04:25] run the agent itself. Then we have Twilio. So, Twilio is a platform that we
[04:27] Twilio. So, Twilio is a platform that we use to buy the number that we're going
[04:29] use to buy the number that we're going to use. I recommend that you're on the
[04:30] to use. I recommend that you're on the plan of pay-as-you-go because numbers
[04:33] plan of pay-as-you-go because numbers obviously are not completely free to
[04:34] obviously are not completely free to use. So, you want to go to the search
[04:36] use. So, you want to go to the search bar, look for phone numbers,
[04:39] bar, look for phone numbers, and then you can go to phone numbers
[04:41] and then you can go to phone numbers here, and then you can go to set up a
[04:43] here, and then you can go to set up a new phone number, choose your country.
[04:44] new phone number, choose your country. Again, not all the countries are going
[04:46] Again, not all the countries are going to be in Twilio, so heads-up. But, the
[04:47] to be in Twilio, so heads-up. But, the ones that we want to use in this case,
[04:49] ones that we want to use in this case, US is fine. You can choose voice as SMS
[04:51] US is fine. You can choose voice as SMS is not what we need right now. Toll-free
[04:53] is not what we need right now. Toll-free is fine, and then you can press search,
[04:55] is fine, and then you can press search, and then you can go through the whole
[04:56] and then you can go through the whole setup of buying the number itself. Put
[04:58] setup of buying the number itself. Put your credit card there, buy it, and then
[04:59] your credit card there, buy it, and then you're good to go. It's only $2.15 per
[05:01] you're good to go. It's only $2.15 per month, so it's all good. It's not going
[05:03] month, so it's all good. It's not going to be anything crazy. Now, once we have
[05:04] to be anything crazy. Now, once we have this, in this case I already have a
[05:05] this, in this case I already have a number here, copy your number and paste
[05:08] number here, copy your number and paste it somewhere safe as well because we're
[05:09] it somewhere safe as well because we're going to add all of these into Claude
[05:11] going to add all of these into Claude code itself. We also want the developer
[05:13] code itself. We also want the developer want to go to API keys and off tokens,
[05:16] want to go to API keys and off tokens, off token,
[05:17] off token, and we want to copy the account SID. And
[05:19] and we want to copy the account SID. And then we also want to copy the primary
[05:21] then we also want to copy the primary off token. So, these are the passwords
[05:22] off token. So, these are the passwords and the way that, you know, Hermes agent
[05:24] and the way that, you know, Hermes agent is able to access the number and it's
[05:26] is able to access the number and it's able for the number to call the lead
[05:28] able for the number to call the lead whenever, you know, a form is submitted.
[05:30] whenever, you know, a form is submitted. All right, so now that we're done with
[05:31] All right, so now that we're done with the actual Twilio, which is the number
[05:32] the actual Twilio, which is the number that we're going to use, we go to Eleven
[05:34] that we're going to use, we go to Eleven Labs. So, Eleven Labs, you can get the
[05:35] Labs. So, Eleven Labs, you can get the $6 a month plan. You can go down to
[05:37] $6 a month plan. You can go down to developers, you can go to API keys, and
[05:39] developers, you can go to API keys, and then you can press create key,
[05:41] then you can press create key, name this Hermes. I'm not going to put
[05:43] name this Hermes. I'm not going to put any restrictions, create a key, and then
[05:45] any restrictions, create a key, and then you have the key right here. We also
[05:46] you have the key right here. We also need a voice, so in this case you can go
[05:47] need a voice, so in this case you can go to voices. You can choose your voice. I
[05:50] to voices. You can choose your voice. I can go to English.
[05:53] can go to English. I can choose American accent.
[05:55] I can choose American accent. And then I can choose
[05:57] And then I can choose &gt;&gt; Hi friends, I'm Lydia.
[05:59] &gt;&gt; Hi friends, I'm Lydia. &gt;&gt; Lydia is fine.
[06:00] &gt;&gt; Lydia is fine. Lydia is fine. We can go here, copy
[06:01] Lydia is fine. We can go here, copy voice ID, and then you can paste it
[06:03] voice ID, and then you can paste it somewhere safe as well. By the way, in
[06:04] somewhere safe as well. By the way, in case you're wondering where I'm looking,
[06:05] case you're wondering where I'm looking, I'm looking at a doc that I'm going to
[06:07] I'm looking at a doc that I'm going to paste everything in. And we're done with
[06:08] paste everything in. And we're done with 11 Labs, we can go to cal.com to set up
[06:10] 11 Labs, we can go to cal.com to set up the event for the voice agent to
[06:11] the event for the voice agent to actually schedule. So you make an
[06:13] actually schedule. So you make an account on cal.com, it's completely
[06:14] account on cal.com, it's completely free. Make an event, in this case I
[06:16] free. Make an event, in this case I called it free property inspection. You
[06:18] called it free property inspection. You can go here, make it 30 minutes. All the
[06:21] can go here, make it 30 minutes. All the settings are pretty standard, you know,
[06:22] settings are pretty standard, you know, availability and so on. And you want to
[06:24] availability and so on. And you want to copy the event ID. So it's the number
[06:26] copy the event ID. So it's the number that you see on the URL on the top.
[06:28] that you see on the URL on the top. Paste the number somewhere, then go to
[06:29] Paste the number somewhere, then go to settings,
[06:30] settings, go to profile, copy your username right
[06:32] go to profile, copy your username right here, cuz we're going to need this. And
[06:34] here, cuz we're going to need this. And then we also want an API key, which you
[06:36] then we also want an API key, which you can find down here, API keys. New,
[06:39] can find down here, API keys. New, Hermis.
[06:41] Hermis. As you can see, I already have four
[06:42] As you can see, I already have four Hermis agents.
[06:43] Hermis agents. Uh press create, and you have the API
[06:45] Uh press create, and you have the API key here. So we're essentially laying
[06:47] key here. So we're essentially laying the foundations for the whole system,
[06:48] the foundations for the whole system, and making sure that the system has
[06:50] and making sure that the system has access to all the softwares that we
[06:51] access to all the softwares that we need. Typically I do do this in the
[06:52] need. Typically I do do this in the first step, just because it doesn't bite
[06:54] first step, just because it doesn't bite me in the ass later, cuz it's much
[06:55] me in the ass later, cuz it's much easier. The next step is Typeform. So
[06:57] easier. The next step is Typeform. So this right here is the software that
[06:58] this right here is the software that we're going to use to be able for the
[07:00] we're going to use to be able for the lead to fill out the form, and then get
[07:01] lead to fill out the form, and then get called in 10 seconds uh by the actual
[07:04] called in 10 seconds uh by the actual system. Go to Typeform, make an account,
[07:06] system. Go to Typeform, make an account, and then you want to create a form. In
[07:07] and then you want to create a form. In this case, the form that I made uh
[07:09] this case, the form that I made uh contains the full name, phone number,
[07:11] contains the full name, phone number, email, services do you need, describe
[07:13] email, services do you need, describe your issue, where is your property
[07:14] your issue, where is your property located, and also when do you need help
[07:16] located, and also when do you need help by? Um that's fine. So make sure you
[07:18] by? Um that's fine. So make sure you have this. All right, once you make the
[07:19] have this. All right, once you make the actual form, we can go to forms here,
[07:21] actual form, we can go to forms here, and we can go to profile, we can go to
[07:23] and we can go to profile, we can go to account settings, we can go to personal
[07:25] account settings, we can go to personal tokens, and we can generate a new token.
[07:28] tokens, and we can generate a new token. Name this Hermis. All scopes is fine.
[07:31] Name this Hermis. All scopes is fine. You can copy this, and that will be your
[07:32] You can copy this, and that will be your API key. All right, the last step is
[07:34] API key. All right, the last step is making a GitHub repository, uh where we
[07:36] making a GitHub repository, uh where we go to make an account, we press new,
[07:38] go to make an account, we press new, name this uh Hermis.
[07:40] name this uh Hermis. And then you can press create a
[07:43] And then you can press create a And then you can copy the link
[07:45] And then you can copy the link right here. And you can keep it. All
[07:47] right here. And you can keep it. All right, with that said, we're done with
[07:48] right, with that said, we're done with getting the API keys and passwords from
[07:50] getting the API keys and passwords from all the softwares. I wanted to show you
[07:51] all the softwares. I wanted to show you that part just because it is actually
[07:53] that part just because it is actually important uh to cover cuz you might be
[07:55] important uh to cover cuz you might be stuck actually finding these yourself.
[07:57] stuck actually finding these yourself. So, I hope that was helpful. Now, let's
[07:58] So, I hope that was helpful. Now, let's go to building the app on Cloud Code and
[08:01] go to building the app on Cloud Code and start there. Hey guys, quick one here.
[08:02] start there. Hey guys, quick one here. If you are working 9:00 to 5:00 and you
[08:04] If you are working 9:00 to 5:00 and you do want to start and scale your own AI
[08:06] do want to start and scale your own AI agency, then check out the first thing
[08:08] agency, then check out the first thing down below which walks you through a
[08:09] down below which walks you through a full video on how you can do that
[08:11] full video on how you can do that step-by-step by working with me
[08:13] step-by-step by working with me one-to-one. Now, let's get back to the
[08:14] one-to-one. Now, let's get back to the video. All right, so the next step is
[08:15] video. All right, so the next step is getting the Google Sheets credentials to
[08:17] getting the Google Sheets credentials to be able for the system to access Google
[08:19] be able for the system to access Google Sheets to add the details of the lead
[08:21] Sheets to add the details of the lead that called in. So, go to
[08:22] that called in. So, go to console.cloud.google.com.
[08:24] console.cloud.google.com. And by the way, all the links are below.
[08:26] And by the way, all the links are below. You can go to select a project. You can
[08:28] You can go to select a project. You can make a new project. In this case, I
[08:29] make a new project. In this case, I already made one. Once you make the
[08:31] already made one. Once you make the name, you should see the name right
[08:32] name, you should see the name right here. It will take about a few seconds
[08:33] here. It will take about a few seconds to to download. You want to go to API
[08:35] to to download. You want to go to API and services. You want to go to the
[08:38] and services. You want to go to the enable API and services and go to Google
[08:40] enable API and services and go to Google Sheet. When you're in the Google Sheets,
[08:41] Sheet. When you're in the Google Sheets, you can press enable and then you're
[08:43] you can press enable and then you're enabling the connection of Google
[08:45] enabling the connection of Google Sheets. The next step is making the
[08:46] Sheets. The next step is making the credentials, so you can go to API and
[08:47] credentials, so you can go to API and services credentials. And then you'll be
[08:49] services credentials. And then you'll be able to create credentials, service
[08:52] able to create credentials, service account. You can name the service
[08:53] account. You can name the service account Hermes 123.
[08:56] account Hermes 123. Create and continue. You can press done.
[08:58] Create and continue. You can press done. Once you made this, you can see it here.
[09:00] Once you made this, you can see it here. I already made two before, so once you
[09:01] I already made two before, so once you go here, you can press this.
[09:04] go here, you can press this. You can copy the email here
[09:06] You can copy the email here that we have. And then we need to go to
[09:07] that we have. And then we need to go to keys. We need to go to add a key.
[09:11] keys. We need to go to add a key. Create a key here, JSON, create it. Uh
[09:13] Create a key here, JSON, create it. Uh and now it might get a bit bit
[09:15] and now it might get a bit bit technical, but don't worry. Um because
[09:17] technical, but don't worry. Um because all we have to do is open the file. So,
[09:18] all we have to do is open the file. So, you should open the actual JSON thing uh
[09:21] you should open the actual JSON thing uh in front of you. By the way, this makes
[09:23] in front of you. By the way, this makes no sense to me whatsoever, so don't
[09:24] no sense to me whatsoever, so don't worry. But, I've just done it so many
[09:25] worry. But, I've just done it so many times that I know what to do here. Uh
[09:27] times that I know what to do here. Uh you can copy everything from here.
[09:30] you can copy everything from here. Let me zoom in. Everything from the
[09:32] Let me zoom in. Everything from the start until the end and paste it
[09:35] start until the end and paste it somewhere safe. And essentially, now
[09:36] somewhere safe. And essentially, now that this is active, this system is able
[09:38] that this is active, this system is able to access our Google Sheet, which is
[09:40] to access our Google Sheet, which is actually the next step. So, we're going
[09:41] actually the next step. So, we're going to go to sheets.new,
[09:43] to go to sheets.new, create a Google Sheet, and then name
[09:45] create a Google Sheet, and then name this lead agent.
[09:48] this lead agent. And we can copy the ID of the sheet
[09:50] And we can copy the ID of the sheet right here, which is the anything from
[09:52] right here, which is the anything from the D until the edit, and paste it
[09:54] the D until the edit, and paste it somewhere. So, we made the folder in
[09:55] somewhere. So, we made the folder in Clock Code. We're going to go here, and
[09:57] Clock Code. We're going to go here, and we're going to paste this prompt right
[09:58] we're going to paste this prompt right here, which tells exactly Clock Code the
[10:00] here, which tells exactly Clock Code the structure of the project, how it's
[10:01] structure of the project, how it's supposed to look, the different files
[10:03] supposed to look, the different files that it's meant to have here, and how
[10:05] that it's meant to have here, and how everything goes together. All right.
[10:06] everything goes together. All right. Now, if you're wondering where you can
[10:07] Now, if you're wondering where you can get the prompt, it's in the second link
[10:09] get the prompt, it's in the second link down below in my free School community.
[10:11] down below in my free School community. You can go to the classroom section,
[10:13] You can go to the classroom section, templates vault, and you'll find
[10:14] templates vault, and you'll find everything there. We have this here. We
[10:15] everything there. We have this here. We can press go, and we can do bypass
[10:17] can press go, and we can do bypass permissions. And by the way, you can
[10:18] permissions. And by the way, you can also screenshot this, and you can give
[10:20] also screenshot this, and you can give it to ChatGPT, and it will give you the
[10:22] it to ChatGPT, and it will give you the prompt itself. As you can see, now it's
[10:23] prompt itself. As you can see, now it's starting to make the actual folders and
[10:24] starting to make the actual folders and files, which is great. All right. It
[10:26] files, which is great. All right. It just finished making the different
[10:27] just finished making the different folders. This is what we call the file
[10:29] folders. This is what we call the file directory, where all the different
[10:30] directory, where all the different folders are and files. I can now paste
[10:32] folders are and files. I can now paste the next prompt, which again, you can
[10:33] the next prompt, which again, you can find in the same document. This right
[10:36] find in the same document. This right here is going to tell it to create a
[10:37] here is going to tell it to create a .env file, not exam, just .env file. So,
[10:40] .env file, not exam, just .env file. So, that we're able to paste all the
[10:41] that we're able to paste all the different credentials and API keys that
[10:43] different credentials and API keys that we just had. And in here, it's saying
[10:45] we just had. And in here, it's saying it, "Hey, when this happens, do this.
[10:46] it, "Hey, when this happens, do this. When this goes wrong, do this." to make
[10:48] When this goes wrong, do this." to make sure that we have those guardrails when
[10:51] sure that we have those guardrails when we're actually doing this. I'm going to
[10:52] we're actually doing this. I'm going to press go, and now it's going to
[10:53] press go, and now it's going to implement the next set of rules. All
[10:55] implement the next set of rules. All right. So, Clock Code just finished
[10:56] right. So, Clock Code just finished making all the different files.
[10:58] making all the different files. &gt;&gt; [music]
[10:58] &gt;&gt; [music] &gt;&gt; One important thing is that we have to
[10:59] &gt;&gt; One important thing is that we have to replace the keys here. So, this is the
[11:01] replace the keys here. So, this is the .env. This is the way that this system
[11:04] .env. This is the way that this system takes all our credentials, all our
[11:05] takes all our credentials, all our passwords, and it uses it to access the
[11:08] passwords, and it uses it to access the softwares when the system is running.
[11:09] softwares when the system is running. So, now we want to paste all the API
[11:11] So, now we want to paste all the API keys that we had before onto here. So,
[11:13] keys that we had before onto here. So, everything from here and out, you should
[11:15] everything from here and out, you should probably have, right? Cuz we went
[11:16] probably have, right? Cuz we went through it before. The one thing that
[11:18] through it before. The one thing that does change is the port, server URL, and
[11:20] does change is the port, server URL, and time zone. So, time zone, you can use
[11:22] time zone. So, time zone, you can use your time zone. In this case, I have
[11:24] your time zone. In this case, I have Asia/Dubai,
[11:25] Asia/Dubai, cuz I'm here right now. Server URL is
[11:28] cuz I'm here right now. Server URL is something you can get by going to the
[11:29] something you can get by going to the Hermes agent, and you can actually just
[11:31] Hermes agent, and you can actually just copy everything from before chat
[11:34] copy everything from before chat up until dot cloud
[11:35] up until dot cloud and you can paste this here.
[11:40] And then the port will be 3000, which is fine. And all of these others we can uh
[11:42] fine. And all of these others we can uh simply just copy this. You can either
[11:44] simply just copy this. You can either copy this here or you can just paste it
[11:46] copy this here or you can just paste it right here. It's blurred right now, but
[11:48] right here. It's blurred right now, but this should be the Anthropic API key, 11
[11:49] this should be the Anthropic API key, 11 Labs, Twilio, call.com, Google,
[11:51] Labs, Twilio, call.com, Google, Typeform, and the server URL, which I
[11:54] Typeform, and the server URL, which I already have, and my phone number as
[11:56] already have, and my phone number as well.
[11:57] well. No, I'm not showing it, so don't try.
[11:59] No, I'm not showing it, so don't try. I'm going to go here, file, save, and
[12:02] I'm going to go here, file, save, and now we're good to go. The next step here
[12:04] now we're good to go. The next step here is to push everything to GitHub. So, we
[12:05] is to push everything to GitHub. So, we want to take all the code and push it
[12:07] want to take all the code and push it here because that is going to be the
[12:08] here because that is going to be the middleman between uh cloud code and
[12:10] middleman between uh cloud code and Hermes agent, right? So, copy this and
[12:13] Hermes agent, right? So, copy this and say
[12:14] say "Hey, I want you to take all this code
[12:15] "Hey, I want you to take all this code and push it directly to GitHub. Here's
[12:18] and push it directly to GitHub. Here's the URL of my repository. Um let me know
[12:20] the URL of my repository. Um let me know when it's done." And we paste the link.
[12:23] when it's done." And we paste the link. There we go. So, now it should take all
[12:24] There we go. So, now it should take all of this, push it to GitHub, so that
[12:27] of this, push it to GitHub, so that Hermes agent is able to read it as well.
[12:29] Hermes agent is able to read it as well. All right, so just as expected, it's
[12:30] All right, so just as expected, it's going to ask me if I want to actually
[12:31] going to ask me if I want to actually push the dot ENV. I'm going to say, "No,
[12:34] push the dot ENV. I'm going to say, "No, don't push the dot ENV, but you can push
[12:36] don't push the dot ENV, but you can push everything else." So, the reason why we
[12:37] everything else." So, the reason why we don't want to push the dot ENV is
[12:39] don't want to push the dot ENV is because our dot ENV contains all our
[12:41] because our dot ENV contains all our service secrets, API keys, passwords. Uh
[12:44] service secrets, API keys, passwords. Uh it's like your credit card number.
[12:45] it's like your credit card number. You're not going to put it in the web,
[12:46] You're not going to put it in the web, right? So, we don't want that going
[12:48] right? So, we don't want that going through. So, very good for asking, and
[12:51] through. So, very good for asking, and uh everything else, yes. All right, so
[12:52] uh everything else, yes. All right, so everything has been pushed to GitHub.
[12:53] everything has been pushed to GitHub. So, if I go here and I refresh, then I
[12:56] So, if I go here and I refresh, then I should see
[12:58] should see different files. Perfect. All right, so
[12:59] different files. Perfect. All right, so the next part is actually using the
[13:00] the next part is actually using the terminal. Now, for those of you who are
[13:02] terminal. Now, for those of you who are not technical at all, this might scare
[13:04] not technical at all, this might scare you, but stay with me. It's actually
[13:05] you, but stay with me. It's actually very, very easy, and I'm going to give
[13:06] very, very easy, and I'm going to give you the full thing to add as well, so
[13:08] you the full thing to add as well, so don't worry. Um cuz I'm also not
[13:10] don't worry. Um cuz I'm also not technical, so we're on the same page
[13:11] technical, so we're on the same page here, okay? We want to go to terminal
[13:13] here, okay? We want to go to terminal here, so make sure you're in Hostinger,
[13:14] here, so make sure you're in Hostinger, go to terminal, and then and then you
[13:16] go to terminal, and then and then you can press and you can type exit here.
[13:19] can press and you can type exit here. And you have the actual thing itself.
[13:20] And you have the actual thing itself. Now, it's listening for any commands or
[13:22] Now, it's listening for any commands or any prompts that we give it. All right,
[13:23] any prompts that we give it. All right, so this right here is going to be the
[13:24] so this right here is going to be the code that we have to paste inside the
[13:26] code that we have to paste inside the terminal right here for it to actually
[13:28] terminal right here for it to actually download everything. So, we need to
[13:29] download everything. So, we need to replace all the keys that we have here.
[13:31] replace all the keys that we have here. So, the first one is the GitHub repo
[13:33] So, the first one is the GitHub repo URL. So, if you remember before, I
[13:35] URL. So, if you remember before, I mentioned to copy the repo URL
[13:37] mentioned to copy the repo URL and paste it here, which I can do right
[13:40] and paste it here, which I can do right now.
[13:41] now. I can paste it here. There we go. And
[13:43] I can paste it here. There we go. And now we have the API keys. So, Anthropic,
[13:45] now we have the API keys. So, Anthropic, Eleven Labs, Voice ID, all that stuff.
[13:47] Eleven Labs, Voice ID, all that stuff. You can go and copy each one. All right,
[13:48] You can go and copy each one. All right, I just pasted all my credentials here.
[13:50] I just pasted all my credentials here. Make sure you go through each one step
[13:51] Make sure you go through each one step by step, and you can also just copy this
[13:53] by step, and you can also just copy this whole thing, paste it into ChatGPT and
[13:55] whole thing, paste it into ChatGPT and ask it, "Hey, what are some things that
[13:56] ask it, "Hey, what are some things that I need to replace?" And you can go back
[13:58] I need to replace?" And you can go back and forth, uh which is fine. And we have
[14:01] and forth, uh which is fine. And we have this at the end as well. Okay? Now, in
[14:03] this at the end as well. Okay? Now, in the doc that I'm going to give you, it
[14:04] the doc that I'm going to give you, it has all the instructions as to what the
[14:06] has all the instructions as to what the network is. It will tell you all the
[14:07] network is. It will tell you all the different naming conventions of these
[14:09] different naming conventions of these different variables that we have to put
[14:10] different variables that we have to put here. Okay? Once you're done, you can
[14:13] here. Okay? Once you're done, you can copy this whole thing. We can go to the
[14:14] copy this whole thing. We can go to the terminal. Again, you can put exit here.
[14:17] terminal. Again, you can put exit here. And the first thing we want to do is we
[14:18] And the first thing we want to do is we want to make a folder and open a file
[14:20] want to make a folder and open a file editor. So, we do this by pasting this
[14:22] editor. So, we do this by pasting this command right here.
[14:23] command right here. Just copy everything step by step. mkdir
[14:26] Just copy everything step by step. mkdir p docker lead up docker lead up and nano
[14:28] p docker lead up docker lead up and nano docker-compose.yml
[14:31] docker-compose.yml um for us to be able to do this. And
[14:33] um for us to be able to do this. And then we can press run. And then here we
[14:35] then we can press run. And then here we can include all the different ENVs and
[14:37] can include all the different ENVs and all the different passwords and keys and
[14:38] all the different passwords and keys and all that stuff. Okay? So, paste this
[14:40] all that stuff. Okay? So, paste this whole thing from the Google Doc that we
[14:41] whole thing from the Google Doc that we made.
[14:42] made. And then once you have this here, you
[14:43] And then once you have this here, you can press control O, enter, control X,
[14:47] can press control O, enter, control X, and we're back here. And then
[14:49] and we're back here. And then all we have to do is paste this right
[14:50] all we have to do is paste this right here. docker-compose up -d. And now you
[14:53] here. docker-compose up -d. And now you can see it's pulling up the document. It
[14:55] can see it's pulling up the document. It says plus
[14:56] says plus 12 out of 12 container lead up. Okay,
[14:58] 12 out of 12 container lead up. Okay, started, pulled. Okay, cool. So, that's
[15:00] started, pulled. Okay, cool. So, that's all good. There's one more thing we have
[15:01] all good. There's one more thing we have to do in terms of the terminal. We going
[15:02] to do in terms of the terminal. We going to have to paste this uh thing right
[15:04] to have to paste this uh thing right here, which is going to allow us to take
[15:06] here, which is going to allow us to take the code, clone it, and actually use it
[15:07] the code, clone it, and actually use it inside the agent. I can press run. All
[15:09] inside the agent. I can press run. All right, cool. So, if you see Hermes agent
[15:11] right, cool. So, if you see Hermes agent listening on 0000 3000, that means that
[15:15] listening on 0000 3000, that means that the agent is now live. So, Hermes agent,
[15:16] the agent is now live. So, Hermes agent, Claude code, GitHub, all the softwares
[15:18] Claude code, GitHub, all the softwares are connected and we're good to go. The
[15:20] are connected and we're good to go. The last step here is the Typeform. So, we
[15:23] last step here is the Typeform. So, we got the scopes, but all we have to do
[15:24] got the scopes, but all we have to do now is actually create the webhook so
[15:26] now is actually create the webhook so that when we fill out the form, it sends
[15:28] that when we fill out the form, it sends the data directly into the system
[15:30] the data directly into the system itself. So, you go to the form, you go
[15:31] itself. So, you go to the form, you go to connect, you go to webhook here. You
[15:35] to connect, you go to webhook here. You press add a webhook. And then here,
[15:37] press add a webhook. And then here, you want to go back to the actual thing
[15:39] you want to go back to the actual thing here. You want to copy everything up
[15:42] here. You want to copy everything up until cloud.
[15:48] Paste it here and put {slash} webhook {slash} Typeform.
[15:50] {slash} Typeform. And press save form or save webhook.
[15:53] And press save form or save webhook. Turn this on.
[15:55] Turn this on. And now, we can test it. Moment of
[15:57] And now, we can test it. Moment of truth. We can test it. If it doesn't
[15:59] truth. We can test it. If it doesn't work, we'll fix it. If it does, amazing.
[16:01] work, we'll fix it. If it does, amazing. All right, finally, we want to paste
[16:02] All right, finally, we want to paste this command right here to watch this
[16:03] this command right here to watch this live and we can now run the actual
[16:06] live and we can now run the actual Typeform. So, we go here, start, name
[16:08] Typeform. So, we go here, start, name this James
[16:09] this James Michael.
[16:11] Michael. Okay, I'll put my phone number here.
[16:12] Okay, I'll put my phone number here. I'll put my email here. Press okay. I
[16:15] I'll put my email here. Press okay. I need roofing services.
[16:17] need roofing services. That's fine. And then, my roof is
[16:20] That's fine. And then, my roof is leaking.
[16:21] leaking. I can press okay. Dallas is fine.
[16:24] I can press okay. Dallas is fine. And then, this week, submit. All right,
[16:27] And then, this week, submit. All right, so as you can see, I'm getting a call
[16:28] so as you can see, I'm getting a call from the agent right now on the screen.
[16:30] from the agent right now on the screen. Hopefully, you guys can hear it.
[16:32] Hopefully, you guys can hear it. &gt;&gt; Hi James Michael. This is Hermes from
[16:33] &gt;&gt; Hi James Michael. This is Hermes from Home Inspection Pros. Thank you for
[16:35] Home Inspection Pros. Thank you for reaching out to us. Can you tell me a
[16:37] reaching out to us. Can you tell me a bit more about the issue or the
[16:39] bit more about the issue or the inspection you need help with?
[16:40] inspection you need help with? &gt;&gt; Yeah, my roof is leaking. It's broken.
[16:42] &gt;&gt; Yeah, my roof is leaking. It's broken. There was a storm the other night. We
[16:43] There was a storm the other night. We need this done ASAP.
[16:44] need this done ASAP. &gt;&gt; Got it. I understand. We'd love to get
[16:47] &gt;&gt; Got it. I understand. We'd love to get an inspector out to you as soon as
[16:49] an inspector out to you as soon as possible. What day and time work best
[16:51] possible. What day and time work best for you?
[16:52] for you? &gt;&gt; Um tomorrow.
[16:53] &gt;&gt; Um tomorrow. &gt;&gt; The earliest available slot I have is
[16:55] &gt;&gt; The earliest available slot I have is Tuesday, June 9th at 3:30 p.m. Would you
[16:58] Tuesday, June 9th at 3:30 p.m. Would you like me to book that for you?
[16:59] like me to book that for you? &gt;&gt; Sure.
[17:00] &gt;&gt; Sure. &gt;&gt; yes or no.
[17:01] &gt;&gt; yes or no. &gt;&gt; Yes. Yes, that's fine.
[17:02] &gt;&gt; Yes. Yes, that's fine. &gt;&gt; You're all set. Your inspection is
[17:04] &gt;&gt; You're all set. Your inspection is booked for Tuesday, June 9th at 3:30
[17:07] booked for Tuesday, June 9th at 3:30 p.m. Our inspector will be there on
[17:09] p.m. Our inspector will be there on time. If you need to reschedule, just
[17:11] time. If you need to reschedule, just give us a call. Have a great day.
[17:12] give us a call. Have a great day. &gt;&gt; Boom, done, finished. Let's go. We can
[17:14] &gt;&gt; Boom, done, finished. Let's go. We can see it on my calendar right here.
[17:16] see it on my calendar right here. Everything else is blurred, but we have
[17:18] Everything else is blurred, but we have the free property inspection at 3:30
[17:20] the free property inspection at 3:30 p.m. my time, James Michael, uh with my
[17:22] p.m. my time, James Michael, uh with my email as well, and my phone number, too.
[17:24] email as well, and my phone number, too. All right, so back to it. There's one
[17:25] All right, so back to it. There's one more thing that I want to show you, uh
[17:27] more thing that I want to show you, uh which is the packaging, pricing, and
[17:29] which is the packaging, pricing, and selling it to businesses. So, one
[17:30] selling it to businesses. So, one important concept about the system is
[17:32] important concept about the system is that it is kind of based on how much a
[17:34] that it is kind of based on how much a lead is worth for the business. Because
[17:36] lead is worth for the business. Because if we are converting more leads, we have
[17:38] if we are converting more leads, we have to understand what is the number, what
[17:40] to understand what is the number, what is the amount that the lead is worth for
[17:42] is the amount that the lead is worth for the business. And so, you can charge
[17:43] the business. And so, you can charge anywhere from 3K all the way to 20K for
[17:46] anywhere from 3K all the way to 20K for the system. Because if a lead is worth a
[17:48] the system. Because if a lead is worth a lot more for Y business than X business,
[17:50] lot more for Y business than X business, you can charge them a lot more. And
[17:52] you can charge them a lot more. And that's exactly how it works with these
[17:53] that's exactly how it works with these kinds of clients. Now, in terms of
[17:55] kinds of clients. Now, in terms of packaging, your offer can literally be
[17:56] packaging, your offer can literally be as simple as we build a speed-to-lead
[17:58] as simple as we build a speed-to-lead voice system that calls and qualifies
[18:00] voice system that calls and qualifies your ad leads in under 5 minutes and
[18:02] your ad leads in under 5 minutes and books them into your calendar without
[18:04] books them into your calendar without hiring extra staff or changing your ad
[18:06] hiring extra staff or changing your ad setup, completely done for you. And like
[18:07] setup, completely done for you. And like I mentioned, in terms of pricing, you
[18:09] I mentioned, in terms of pricing, you can either charge per booked appointment
[18:11] can either charge per booked appointment or you can charge a setup fee plus a
[18:13] or you can charge a setup fee plus a monthly retainer, which can be anywhere
[18:15] monthly retainer, which can be anywhere from a 3K setup fee to a 10K setup fee
[18:18] from a 3K setup fee to a 10K setup fee and a 1 to 2K a month retainer as well,
[18:20] and a 1 to 2K a month retainer as well, depending on the business. All right, so
[18:21] depending on the business. All right, so there we there is a full build. You've
[18:22] there we there is a full build. You've now taken two softwares and you have
[18:25] now taken two softwares and you have built a working agents that businesses
[18:27] built a working agents that businesses are actually willing to pay thousands of
[18:29] are actually willing to pay thousands of dollars for. Uh but that's the build,
[18:31] dollars for. Uh but that's the build, right? The build is the easy part. The
[18:32] right? The build is the easy part. The hard part is not so much can we build
[18:34] hard part is not so much can we build this, but it's can we sell this and get
[18:35] this, but it's can we sell this and get clients, which is exactly what I'm
[18:37] clients, which is exactly what I'm covering in my free live workshop later
[18:39] covering in my free live workshop later this month. If you want to check it out,
[18:41] this month. If you want to check it out, you can check it out in the second link
[18:42] you can check it out in the second link down below. It'll be there. You can
[18:44] down below. It'll be there. You can apply and I'll see you there. All right,
[18:45] apply and I'll see you there. All right, and once again, if you guys want the
[18:47] and once again, if you guys want the resources and templates and prompts of
[18:49] resources and templates and prompts of this whole video, then check it out in
[18:51] this whole video, then check it out in the link down below in my free school
[18:53] the link down below in my free school community. Uh you can go to the
[18:54] community. Uh you can go to the classroom section, you can go to the
[18:56] classroom section, you can go to the templates vault, and you'll find
[18:57] templates vault, and you'll find everything there. And now that you've
[18:58] everything there. And now that you've actually built and packaged one specific
[19:00] actually built and packaged one specific offer for a business, check out this
[19:02] offer for a business, check out this video on the screen where I show you 20
[19:04] video on the screen where I show you 20 AI agency offers that actually make
[19:06] AI agency offers that actually make money in 2026. With that being said, I
[19:08] money in 2026. With that being said, I really hope you guys found value from
[19:10] really hope you guys found value from this video, and as always, I'll see you
[19:12] this video, and as always, I'll see you in the next one.

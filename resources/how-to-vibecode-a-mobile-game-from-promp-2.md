Source: https://youtu.be/mDQOfUHz07U?si=AV37Re3mwmAy0dVB
Title: How to Vibecode a Mobile Game — From Prompt to APK in 1.5h
Duration: 07:24 (443.7s)
Transcript source: captions
============================================================

[00:01] Hey guys, so today I want to show you something pretty cool. I made [music] a
[00:03] something pretty cool. I made [music] a game. It's very similar to Fruit Merge,
[00:05] game. It's very similar to Fruit Merge, which has something like 10 million
[00:07] which has something like 10 million downloads on Google Play right now. I
[00:09] downloads on Google Play right now. I wanted to see if a new AI tool could
[00:11] wanted to see if a new AI tool could basically recreate it. Like, can it
[00:14] basically recreate it. Like, can it actually handle 2D physics, in-app
[00:16] actually handle 2D physics, in-app purchases, appropriate APK you can
[00:18] purchases, appropriate APK you can upload to a store? And later in this
[00:20] upload to a store? And later in this video, I'll tell you how long it took to
[00:22] video, I'll tell you how long it took to web code the whole thing for me. So, let
[00:24] web code the whole thing for me. So, let me just show you how it went. If you
[00:26] me just show you how it went. If you follow this channel, you know that I've
[00:28] follow this channel, you know that I've been doing web coding for 6 or 7 months
[00:30] been doing web coding for 6 or 7 months now. And honestly, I'm kind of hooked on
[00:32] now. And honestly, I'm kind of hooked on it. The idea is pretty simple. Instead
[00:34] it. The idea is pretty simple. Instead of writing code yourself, you just
[00:36] of writing code yourself, you just describe what you want, and the AI
[00:38] describe what you want, and the AI figures out the technical side. For this
[00:40] figures out the technical side. For this project, I used Abacus AI Deep Agent,
[00:42] project, I used Abacus AI Deep Agent, which is also a sponsor of this video.
[00:44] which is also a sponsor of this video. I'll talk more about it later. But by
[00:47] I'll talk more about it later. But by the way, the link is in the description
[00:48] the way, the link is in the description of the video. One thing I did before
[00:51] of the video. One thing I did before starting, since Fruit Merge already
[00:53] starting, since Fruit Merge already exists, I just asked Claude to look at
[00:56] exists, I just asked Claude to look at it and write proper game spec for me,
[00:58] it and write proper game spec for me, [music] breaking down all the game rules
[01:00] [music] breaking down all the game rules and systems. Why sit and write it all
[01:02] and systems. Why sit and write it all out when the reference game is right
[01:05] out when the reference game is right there? So, yeah, one AI wrote the prompt
[01:07] there? So, yeah, one AI wrote the prompt for another AI. That's just where we are
[01:10] for another AI. That's just where we are now, I guess.
[01:11] now, I guess. &gt;&gt; [music]
[01:11] &gt;&gt; [music] &gt;&gt; The game is called, you will never
[01:13] &gt;&gt; The game is called, you will never guess, the Vegetable Merge. Very
[01:15] guess, the Vegetable Merge. Very original, I know. Same idea as Suika
[01:18] original, I know. Same idea as Suika Game or Fruit Merge. You drop things
[01:20] Game or Fruit Merge. You drop things into a box, and when two of the same
[01:22] into a box, and when two of the same kind touch, they merge into a bigger
[01:24] kind touch, they merge into a bigger one. Except instead of fruits, it's
[01:26] one. Except instead of fruits, it's vegetables. [music]
[01:27] vegetables. [music] Starts with a P, ends with a giant
[01:30] Starts with a P, ends with a giant pumpkin. The thing is, even a simple
[01:32] pumpkin. The thing is, even a simple game like this has quite a few pieces.
[01:35] game like this has quite a few pieces. You need physics, collision, scoring,
[01:37] You need physics, collision, scoring, sounds, a game over screen, and if you
[01:40] sounds, a game over screen, and if you actually want to monetize it, in-app
[01:42] actually want to monetize it, in-app purchases on both iOS and Android.
[01:45] purchases on both iOS and Android. That's normally not a quick thing to put
[01:47] That's normally not a quick thing to put together. So, I sent this back over to
[01:49] together. So, I sent this back over to Abacus AI Deep Agent, and within a few
[01:51] Abacus AI Deep Agent, and within a few minutes, it had built out a full React
[01:54] minutes, it had built out a full React Native code, physics engine, scoring,
[01:56] Native code, physics engine, scoring, high score saved locally, even generated
[01:59] high score saved locally, even generated three sound effects, drop sound, merge
[02:01] three sound effects, drop sound, merge sound, game over jingle. All from one
[02:04] sound, game over jingle. All from one prompt. All the prompts are in the
[02:06] prompt. All the prompts are in the description if you want to try this
[02:08] description if you want to try this yourself. Abacus AI Deep Agent actually
[02:10] yourself. Abacus AI Deep Agent actually asked me up front whether I wanted to
[02:12] asked me up front whether I wanted to use its default graphics and music or
[02:15] use its default graphics and music or make my own. For the music, I just kept
[02:17] make my own. For the music, I just kept what it suggested, sounded fine to me.
[02:20] what it suggested, sounded fine to me. But for the graphics, I wanted something
[02:22] But for the graphics, I wanted something custom. It wasn't perfect straight away,
[02:24] custom. It wasn't perfect straight away, by the way. The colliders were just
[02:26] by the way. The colliders were just circles for every vegetable, so a carrot
[02:28] circles for every vegetable, so a carrot was basically bouncing like a ball. And
[02:30] was basically bouncing like a ball. And everything was a bit small. But I just
[02:33] everything was a bit small. But I just told it to fix those things, and it did.
[02:35] told it to fix those things, and it did. Make the colliders match the actual
[02:37] Make the colliders match the actual shape. Make all vegetables twice as big.
[02:40] shape. Make all vegetables twice as big.
[02:40] &gt;&gt; [music] &gt;&gt; Done. So, for the graphics, I want to
[02:42] &gt;&gt; Done. So, for the graphics, I want to Chat LLM, which has Nana Banana 2 and
[02:45] Chat LLM, which has Nana Banana 2 and Nana Banana Pro built in, alongside with
[02:48] Nana Banana Pro built in, alongside with Code and bunch of other models. And
[02:50] Code and bunch of other models. And [music] at this point, I'm just using
[02:52] [music] at this point, I'm just using Claude to talk to everything else. And
[02:55] Claude to talk to everything else. And then generated all 10 vegetables in one
[02:58] then generated all 10 vegetables in one image, same style, same lighting, all on
[03:00] image, same style, same lighting, all on one sheet. Then cut them out in
[03:02] one sheet. Then cut them out in Photoshop and saved each one as PNG.
[03:05] Photoshop and saved each one as PNG. That part took maybe 20 minutes. Then I
[03:08] That part took maybe 20 minutes. Then I just uploaded them to Abacus AI Deep
[03:10] just uploaded them to Abacus AI Deep Agent and said, "Here are the images,
[03:12] Agent and said, "Here are the images, put them in the game, and also make the
[03:14] put them in the game, and also make the vegetables spin when they fall." And it
[03:17] vegetables spin when they fall." And it works really nicely. The spinning is
[03:19] works really nicely. The spinning is driven by the physics engine, so when a
[03:21] driven by the physics engine, so when a carrot hits a wall, it rotates based on
[03:24] carrot hits a wall, it rotates based on actual collision, not a fake animation,
[03:26] actual collision, not a fake animation, real physics. [music]
[03:27] real physics. [music] After that, I did a few more small
[03:29] After that, I did a few more small things. Asked for a cartoonish font, got
[03:33] things. Asked for a cartoonish font, got Fredoka One, which looks great. Asked
[03:35] Fredoka One, which looks great. Asked for an aquarium background, it added the
[03:37] for an aquarium background, it added the water gradient and floating bubbles, and
[03:39] water gradient and floating bubbles, and tweaked the physics a bit to make the
[03:41] tweaked the physics a bit to make the vegetables feel bouncier and stack
[03:43] vegetables feel bouncier and stack tighter. Each of those was one prompt.
[03:46] tighter. Each of those was one prompt. Now, let's check whether we can
[03:47] Now, let's check whether we can implement in-app purchases. IAPs are
[03:50] implement in-app purchases. IAPs are usually a pain. iOS and Android has
[03:52] usually a pain. iOS and Android has completely different systems, and you
[03:54] completely different systems, and you have to handle a bunch of edge cases
[03:56] have to handle a bunch of edge cases like failed purchases, canceled
[03:58] like failed purchases, canceled purchases, the billing connection drop
[04:01] purchases, the billing connection drop in. It's one of those things that always
[04:03] in. It's one of those things that always takes longer than you think, unless you
[04:05] takes longer than you think, unless you have the code that you can reuse in all
[04:07] have the code that you can reuse in all of your games. That's what I normally
[04:09] of your games. That's what I normally do. I asked for a simple continue
[04:11] do. I asked for a simple continue purchase, 99 cents. It clears all the
[04:14] purchase, 99 cents. It clears all the vegetables that are above the danger
[04:16] vegetables that are above the danger line, so you can keep going without
[04:18] line, so you can keep going without losing your score. It built a purchase
[04:20] losing your score. It built a purchase manager class that keeps all the iOS and
[04:23] manager class that keeps all the iOS and Android billing stuff separate from the
[04:25] Android billing stuff separate from the game code. The game just calls one
[04:27] game code. The game just calls one function and reacts to the result.
[04:30] function and reacts to the result. Uh there was one bug. It crashed on the
[04:32] Uh there was one bug. It crashed on the web preview because IAPs don't work in
[04:35] web preview because IAPs don't work in browser. Honestly, I would have figured
[04:37] browser. Honestly, I would have figured it that out pretty quickly myself, but
[04:40] it that out pretty quickly myself, but the fix was just one prompt away, so not
[04:42] the fix was just one prompt away, so not a big deal. So, Abacus AI Deep Agent is
[04:44] a big deal. So, Abacus AI Deep Agent is sponsoring this video, and I'll tell you
[04:46] sponsoring this video, and I'll tell you what actually stood out to me about it.
[04:48] what actually stood out to me about it. Most AI tools just get back some code,
[04:51] Most AI tools just get back some code, and then you have to run it yourself and
[04:53] and then you have to run it yourself and figure out what broke. With Abacus AI
[04:56] figure out what broke. With Abacus AI Deep Agent, it it has a live environment
[04:58] Deep Agent, it it has a live environment where it actually runs the code, sees
[05:00] where it actually runs the code, sees the errors, and fixes them on its own.
[05:02] the errors, and fixes them on its own. The back-and-forth loop is actually
[05:04] The back-and-forth loop is actually where you lose a lot of time. And what I
[05:06] where you lose a lot of time. And what I really like that it isn't just useful
[05:08] really like that it isn't just useful for prototype. You can actually take
[05:10] for prototype. You can actually take what it builds, create a game, page on
[05:12] what it builds, create a game, page on Google Play, set up the in-app
[05:14] Google Play, set up the in-app purchases, and publish it straight away.
[05:16] purchases, and publish it straight away. Like, what you are watching right now is
[05:17] Like, what you are watching right now is basically a shippable game. And it's not
[05:20] basically a shippable game. And it's not just games. They've built expenses
[05:22] just games. They've built expenses trackers, fitness apps, social feeds,
[05:25] trackers, fitness apps, social feeds, all from single prompts with real
[05:26] all from single prompts with real databases behind them. You can get
[05:28] databases behind them. You can get started for 10 bucks a month, and link
[05:31] started for 10 bucks a month, and link in the description of the video. And
[05:32] in the description of the video. And let's test it out on the device. So, I
[05:35] let's test it out on the device. So, I opened Expo Go on my iPhone, scanned the
[05:37] opened Expo Go on my iPhone, scanned the QR code, and the game just loaded on my
[05:40] QR code, and the game just loaded on my phone. Custom art, physics, sound, and
[05:42] phone. Custom art, physics, sound, and bubbles in the background, everything.
[05:44] bubbles in the background, everything. No Xcode, no build setup, just scan and
[05:47] No Xcode, no build setup, just scan and it works. Uh then I built the APK for
[05:50] it works. Uh then I built the APK for Android phone. It was 154 megabytes, by
[05:53] Android phone. It was 154 megabytes, by the way. Just downloaded it to my
[05:54] the way. Just downloaded it to my Android phone, and it runs great. And
[05:57] Android phone, and it runs great. And the whole thing from start to finish
[05:59] the whole thing from start to finish took me about 1 and 1/2 hours. That
[06:01] took me about 1 and 1/2 hours. That included the art in Chat LLM, cutting
[06:03] included the art in Chat LLM, cutting out in Photoshop, all prompts,
[06:05] out in Photoshop, all prompts, everything. Is it fully ready to submit
[06:07] everything. Is it fully ready to submit to stores? Not quite. I still need to
[06:09] to stores? Not quite. I still need to set up the IAP products in the App Store
[06:12] set up the IAP products in the App Store Connect and Play Store, [music]
[06:13] Connect and Play Store, [music] write some store listing, do proper
[06:15] write some store listing, do proper testing, create screenshots, create
[06:18] testing, create screenshots, create icons. But the actual game is done and
[06:20] icons. But the actual game is done and working. I think the interesting shift
[06:22] working. I think the interesting shift here isn't really about speed. It's
[06:24] here isn't really about speed. It's about what actually requires your input
[06:27] about what actually requires your input now. The physics stuff, the billing
[06:28] now. The physics stuff, the billing code, the font loading, that stuff is
[06:31] code, the font loading, that stuff is just handled. What still needs you is
[06:33] just handled. What still needs you is knowing if the game feels good, whether
[06:35] knowing if the game feels good, whether the art fits, whether the core loop is
[06:37] the art fits, whether the core loop is fun to play. That part the AI can't do
[06:39] fun to play. That part the AI can't do for you. So, for me, web coding is not
[06:42] for you. So, for me, web coding is not really a shortcut anymore. It's more
[06:44] really a shortcut anymore. It's more like a different way of working, where
[06:45] like a different way of working, where you focus more on the decisions. All the
[06:48] you focus more on the decisions. All the prompts from this are in the description
[06:49] prompts from this are in the description of this video, along with the link for
[06:51] of this video, along with the link for Abacus AI Deep Agent. [music]
[06:53] Abacus AI Deep Agent. [music] The full game spec, the IAP prompt,
[06:55] The full game spec, the IAP prompt, everything. Feel free to take them and
[06:57] everything. Feel free to take them and build something different. If you want
[06:58] build something different. If you want to see the next part, getting it onto
[07:01] to see the next part, getting it onto stores, setting up the IAP products,
[07:04] stores, setting up the IAP products, going through App Store review, let me
[07:05] going through App Store review, let me know in the comments, and I'll make a
[07:07] know in the comments, and I'll make a video for that. Subscribe if you're into
[07:10] video for that. Subscribe if you're into this kind of stuff, and I'll see you in
[07:11] this kind of stuff, and I'll see you in the next one. Bye.

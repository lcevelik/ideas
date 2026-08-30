Source: https://youtu.be/wZYD-zTKe7Y?si=MhigZMwhMqzPsXH0
Title: Claude Code Just Made Fine-Tuning AI Images Stupid Easy!
Duration: 19:02 (1141.5s)
Transcript source: captions
============================================================

[00:01] AI image models are incredible at giving you something new every time you prompt
[00:04] you something new every time you prompt them, but that's also their biggest
[00:06] them, but that's also their biggest weakness. You see, the moment you need
[00:08] weakness. You see, the moment you need to make the same thing twice, the same
[00:10] to make the same thing twice, the same character, the same product, your own
[00:11] character, the same product, your own face, it starts to get distorted and
[00:14] face, it starts to get distorted and lose the details. But the good news is
[00:16] lose the details. But the good news is that you don't have to sit around
[00:17] that you don't have to sit around waiting for some company to fix it,
[00:18] waiting for some company to fix it, because you can train your own AI image
[00:20] because you can train your own AI image model that locks onto exactly what you
[00:22] model that locks onto exactly what you want and recreates it perfectly every
[00:24] want and recreates it perfectly every single time. Now, you might be
[00:26] single time. Now, you might be wondering, why can't I just use Nanao
[00:27] wondering, why can't I just use Nanao Banana Pro and give it a reference image
[00:29] Banana Pro and give it a reference image instead? Well, a reference image only
[00:31] instead? Well, a reference image only knows that one photo. A new angle, a new
[00:34] knows that one photo. A new angle, a new pose, different lighting, and it starts
[00:36] pose, different lighting, and it starts guessing, which is exactly where those
[00:38] guessing, which is exactly where those details fall apart. And that's where in
[00:39] details fall apart. And that's where in this video, we are going to create our
[00:41] this video, we are going to create our own custom Laura, which means Laura and
[00:43] own custom Laura, which means Laura and character adaptation. And I know you
[00:45] character adaptation. And I know you might be thinking, this sounds super
[00:46] might be thinking, this sounds super complicated. I might need years of
[00:48] complicated. I might need years of experience to actually do this, but no,
[00:49] experience to actually do this, but no, you really don't, because I'll walk you
[00:51] you really don't, because I'll walk you through the entire process. And we're
[00:53] through the entire process. And we're even going to get Claude code to hold
[00:54] even going to get Claude code to hold our hand throughout it. Also, you don't
[00:57] our hand throughout it. Also, you don't need a $5,000 graphics card for this,
[00:59] need a $5,000 graphics card for this, because we're going to use a cloud
[01:00] because we're going to use a cloud computer for just $2. Now, once we have
[01:03] computer for just $2. Now, once we have our Laura, we are going to generate our
[01:04] our Laura, we are going to generate our own images inside of ComfyUI. And
[01:07] own images inside of ComfyUI. And ComfyUI is basically the most powerful
[01:09] ComfyUI is basically the most powerful generative AI tool. It's based on nodes,
[01:11] generative AI tool. It's based on nodes, which you can think of like blocks, and
[01:13] which you can think of like blocks, and you can connect many different blocks to
[01:15] you can connect many different blocks to create an entire workflow. And these
[01:17] create an entire workflow. And these nodes can do a lot, like load the AI
[01:19] nodes can do a lot, like load the AI models, encode text prompts, set image
[01:21] models, encode text prompts, set image dimensions, run the diffusion sampling
[01:23] dimensions, run the diffusion sampling process, a bunch more stuff that we
[01:25] process, a bunch more stuff that we don't really need to understand, because
[01:26] don't really need to understand, because we won't even touch any of it. Because
[01:28] we won't even touch any of it. Because as I said, this is going to be our
[01:30] as I said, this is going to be our workflow. We're just going to talk in
[01:32] workflow. We're just going to talk in plain English. If you can speak English,
[01:34] plain English. If you can speak English, you can do this. To Claude code and get
[01:35] you can do this. To Claude code and get it to set up all these complicated
[01:37] it to set up all these complicated workflows and nodes for us. And there's
[01:39] workflows and nodes for us. And there's also many more less known benefits of
[01:42] also many more less known benefits of using ComfyUI as opposed to a regular
[01:44] using ComfyUI as opposed to a regular image model, such as your generations
[01:46] image model, such as your generations can be entirely free, because ComfyUI is
[01:49] can be entirely free, because ComfyUI is open source, meaning you can run it on
[01:50] open source, meaning you can run it on your own hardware. You can also have far
[01:52] your own hardware. You can also have far more customizability. These are just
[01:54] more customizability. These are just three very simple examples, but it goes
[01:56] three very simple examples, but it goes to show how many little tweaks and
[01:58] to show how many little tweaks and details that you can actually change
[01:59] details that you can actually change since you have full control over this
[02:01] since you have full control over this process. And since I'll be showing you
[02:03] process. And since I'll be showing you how to actually create a Laura to
[02:04] how to actually create a Laura to fine-tune these AI image models, you can
[02:06] fine-tune these AI image models, you can build characters that stay identical
[02:08] build characters that stay identical across the entire workflow. And since
[02:10] across the entire workflow. And since you can run it locally, meaning on your
[02:12] you can run it locally, meaning on your own computer, your data and your face
[02:14] own computer, your data and your face never need to touch someone else's
[02:15] never need to touch someone else's servers, and also it works offline. And
[02:18] servers, and also it works offline. And even if you have a really terrible PC,
[02:20] even if you have a really terrible PC, all of the skills that you learn can
[02:21] all of the skills that you learn can still be applied because you can just
[02:23] still be applied because you can just use Comfy Cloud, which is the cloud
[02:25] use Comfy Cloud, which is the cloud version of Comfy UI. So, in this video,
[02:27] version of Comfy UI. So, in this video, I'm about to show you how to create your
[02:28] I'm about to show you how to create your own custom Laura for less than $2. We're
[02:31] own custom Laura for less than $2. We're going to wire up Comfy UI without
[02:32] going to wire up Comfy UI without touching a single node. I'll explain
[02:34] touching a single node. I'll explain everything you need to know about this
[02:36] everything you need to know about this process in super simple terms, even if
[02:38] process in super simple terms, even if you're a complete beginner. And if you
[02:39] you're a complete beginner. And if you follow along and actually watch until
[02:41] follow along and actually watch until the end, you're going to be able to set
[02:42] the end, you're going to be able to set everything up yourself and recreate your
[02:44] everything up yourself and recreate your subject perfectly every single time. So,
[02:47] subject perfectly every single time. So, let's get into it. So, the first step
[02:48] let's get into it. So, the first step we're going to do is just download Comfy
[02:50] we're going to do is just download Comfy UI. I'm going to leave a link for it in
[02:51] UI. I'm going to leave a link for it in the description, but it's super simple.
[02:53] the description, but it's super simple. Open the link in the description and
[02:54] Open the link in the description and then click on download desktop. This
[02:56] then click on download desktop. This video is not sponsored, by the way. The
[02:58] video is not sponsored, by the way. The second step is to install Cloud Code.
[03:00] second step is to install Cloud Code. Cloud Code is basically a powerful AI
[03:01] Cloud Code is basically a powerful AI agent that you can give access to your
[03:03] agent that you can give access to your PC so that it can perform tasks, and not
[03:06] PC so that it can perform tasks, and not just generate responses or chat with
[03:08] just generate responses or chat with you. It can actually take actions on
[03:10] you. It can actually take actions on your computer, which is why we're going
[03:12] your computer, which is why we're going to connect it to Comfy UI. So, to
[03:14] to connect it to Comfy UI. So, to install Cloud Code, I'm going to leave
[03:15] install Cloud Code, I'm going to leave another link in the description, and you
[03:17] another link in the description, and you can just scroll down here to install
[03:18] can just scroll down here to install Cloud Code, and just download it for
[03:19] Cloud Code, and just download it for whatever operating system you have. So,
[03:21] whatever operating system you have. So, I have Mac, so I can copy this. I'm
[03:23] I have Mac, so I can copy this. I'm going to press command space and type in
[03:25] going to press command space and type in terminal. Once I have my terminal opened
[03:27] terminal. Once I have my terminal opened up, I'm just going to paste the command
[03:28] up, I'm just going to paste the command that I just copied from the Cloud Code
[03:30] that I just copied from the Cloud Code docs and hit enter. And this is going to
[03:32] docs and hit enter. And this is going to bring you through a bunch of different
[03:34] bring you through a bunch of different steps, super simple, super easy to
[03:36] steps, super simple, super easy to follow along to actually install Cloud
[03:37] follow along to actually install Cloud Code. You can give it all the
[03:38] Code. You can give it all the permissions, and if you have a Cloud
[03:40] permissions, and if you have a Cloud subscription, you can also use that to
[03:43] subscription, you can also use that to power Cloud Code, so you don't need to
[03:44] power Cloud Code, so you don't need to spend money on extra credits or tokens
[03:46] spend money on extra credits or tokens if you're already paying for the $20 a
[03:48] if you're already paying for the $20 a month subscription. So, by this point,
[03:49] month subscription. So, by this point, you should have Comfy UI downloaded and
[03:51] you should have Comfy UI downloaded and Cloud Code installed. So, what we're
[03:52] Cloud Code installed. So, what we're going to do now is just open up ComfyUI,
[03:54] going to do now is just open up ComfyUI, which might take a few minutes since
[03:56] which might take a few minutes since again this is running locally on my
[03:58] again this is running locally on my machine. Now, while Cloud is starting,
[04:00] machine. Now, while Cloud is starting, I'm going to open up Cloud Code. So, if
[04:01] I'm going to open up Cloud Code. So, if you close it for whatever reason, you
[04:03] you close it for whatever reason, you can reopen it, and I recommend reopening
[04:05] can reopen it, and I recommend reopening it by typing cloud {dash} {dash}
[04:06] it by typing cloud {dash} {dash} dangerously {dash} skip {dash}
[04:09] dangerously {dash} skip {dash} permissions because this is basically
[04:10] permissions because this is basically going to give it full access, and you're
[04:11] going to give it full access, and you're not going to need to approve every
[04:13] not going to need to approve every single command that comes up. For the
[04:14] single command that comes up. For the model, I recommend always going with the
[04:16] model, I recommend always going with the most powerful model. So, whenever you're
[04:18] most powerful model. So, whenever you're watching this video, Fable 5 might be
[04:20] watching this video, Fable 5 might be re-released, so maybe use that. For now,
[04:22] re-released, so maybe use that. For now, I have {slash} model click, and you can
[04:24] I have {slash} model click, and you can change the model. So, I have Opus 4.8
[04:26] change the model. So, I have Opus 4.8 selected with 1 million context, so we
[04:28] selected with 1 million context, so we can select that. And then {slash}
[04:29] can select that. And then {slash} effort, we can change it to whatever we
[04:31] effort, we can change it to whatever we want, basically how much thinking Cloud
[04:33] want, basically how much thinking Cloud Code is going to have to do. So, I'm
[04:34] Code is going to have to do. So, I'm just going to leave it at high. And as
[04:35] just going to leave it at high. And as you can see this red text, bypass
[04:37] you can see this red text, bypass permissions is on. So, ultimately this
[04:39] permissions is on. So, ultimately this is the most risky approach because it's
[04:42] is the most risky approach because it's going to be able to do whatever it wants
[04:43] going to be able to do whatever it wants on your PC. This is where I recommend
[04:44] on your PC. This is where I recommend using the most powerful models so that
[04:46] using the most powerful models so that it makes the least mistakes. Now, I had
[04:48] it makes the least mistakes. Now, I had a ComfyUI update, so this me a bit
[04:50] a ComfyUI update, so this me a bit longer than usual, but I just want to
[04:51] longer than usual, but I just want to say before we do open up ComfyUI, if you
[04:54] say before we do open up ComfyUI, if you want all of the prompts that I'm using,
[04:56] want all of the prompts that I'm using, then I'm going to leave all of the
[04:57] then I'm going to leave all of the resources completely for free in the
[04:58] resources completely for free in the Editing Evolution. It's going to be the
[05:00] Editing Evolution. It's going to be the first link below the video where you can
[05:01] first link below the video where you can just copy all these prompts that I'm
[05:02] just copy all these prompts that I'm using throughout this video and paste
[05:03] using throughout this video and paste them into your workflows. Okay, now we
[05:05] them into your workflows. Okay, now we have ComfyUI opened up on the desktop
[05:08] have ComfyUI opened up on the desktop app. The only things I want to show you
[05:09] app. The only things I want to show you is up the top left, you can click on
[05:10] is up the top left, you can click on plus to create and delete workflows. You
[05:13] plus to create and delete workflows. You can right click anywhere if you want to
[05:14] can right click anywhere if you want to add any nodes, which is the thing we
[05:15] add any nodes, which is the thing we talked about earlier, but again, you
[05:17] talked about earlier, but again, you don't even need to do this. But okay,
[05:18] don't even need to do this. But okay, now it's time to connect Cloud Code to
[05:20] now it's time to connect Cloud Code to ComfyUI. So, we have this prompt ready
[05:22] ComfyUI. So, we have this prompt ready right here, which has a link to this
[05:23] right here, which has a link to this GitHub, which basically allows you to
[05:25] GitHub, which basically allows you to create your ComfyUI instance as a server
[05:28] create your ComfyUI instance as a server so that it's easier to connect Cloud
[05:30] so that it's easier to connect Cloud Code to it. You don't need to understand
[05:31] Code to it. You don't need to understand any of this, by the way. That's why I'm
[05:32] any of this, by the way. That's why I'm just linking it to Cloud Code. So, now
[05:34] just linking it to Cloud Code. So, now we're going to open up the terminal
[05:35] we're going to open up the terminal again, paste in the prompt we just sent,
[05:37] again, paste in the prompt we just sent, and we're just going to send that off.
[05:38] and we're just going to send that off. Now, once that's sending, let me explain
[05:40] Now, once that's sending, let me explain a bit more of the prompt. So, firstly,
[05:41] a bit more of the prompt. So, firstly, this is called a GitHub repository. So,
[05:44] this is called a GitHub repository. So, we're saying to analyze this repository,
[05:46] we're saying to analyze this repository, copy it, and install the skill. And
[05:48] copy it, and install the skill. And then, I'm not even telling my Mac where
[05:50] then, I'm not even telling my Mac where the ComfyUI is installed, I'm telling it
[05:52] the ComfyUI is installed, I'm telling it to find the Mac ComfyUI installation
[05:55] to find the Mac ComfyUI installation path automatically and adapt all the
[05:57] path automatically and adapt all the paths to it, and also to not assume a
[05:59] paths to it, and also to not assume a Windows path. So, this part you need to
[06:01] Windows path. So, this part you need to change if you do have a Windows and you
[06:03] change if you do have a Windows and you don't have a Mac. Now, I'm also telling
[06:05] don't have a Mac. Now, I'm also telling it to not download any models and not
[06:07] it to not download any models and not build any pipelines yet, because we're
[06:08] build any pipelines yet, because we're going to do that later. I just wanted to
[06:10] going to do that later. I just wanted to set up the skill and connect it to my
[06:12] set up the skill and connect it to my local ComfyUI. That's all this prompt
[06:13] local ComfyUI. That's all this prompt is. So, as you can see, it says
[06:15] is. So, as you can see, it says connection confirmed. It found 1,024
[06:17] connection confirmed. It found 1,024 node types, and now it's writing the
[06:18] node types, and now it's writing the adapted skill. And as you can see,
[06:20] adapted skill. And as you can see, that's how easy it is to connect ComfyUI
[06:22] that's how easy it is to connect ComfyUI to CloudCode. Now, I'm about to show you
[06:24] to CloudCode. Now, I'm about to show you how you can actually fine-tune an image
[06:26] how you can actually fine-tune an image model so that you can get these
[06:27] model so that you can get these consistent results. And keep in mind,
[06:29] consistent results. And keep in mind, you can be creative with this, because
[06:30] you can be creative with this, because you can make money by doing this for
[06:32] you can make money by doing this for brands, for companies, for influencers,
[06:34] brands, for companies, for influencers, whatever it is. Companies have strict
[06:36] whatever it is. Companies have strict policies, especially for marketing and
[06:38] policies, especially for marketing and for showing off their products. So, you
[06:39] for showing off their products. So, you can't have inconsistent images if you're
[06:41] can't have inconsistent images if you're doing a marketing campaign. That's why
[06:43] doing a marketing campaign. That's why this is actually a valuable skill to
[06:45] this is actually a valuable skill to learn. For influencers, you want to make
[06:46] learn. For influencers, you want to make sure that their face is actually
[06:47] sure that their face is actually consistent, whether it's for thumbnails,
[06:49] consistent, whether it's for thumbnails, whether it's for X posts, blog posts,
[06:51] whether it's for X posts, blog posts, Instagram, whatever it is. So, you can
[06:52] Instagram, whatever it is. So, you can offer them this kind of a service.
[06:54] offer them this kind of a service. Again, there's lots of different use
[06:55] Again, there's lots of different use cases that you can actually use this
[06:57] cases that you can actually use this for. So, I highly recommend that you
[06:59] for. So, I highly recommend that you just stay open-minded when watching this
[07:00] just stay open-minded when watching this video and see what use cases you can
[07:03] video and see what use cases you can find for yourself. Reason being, because
[07:05] find for yourself. Reason being, because I use my face a lot in thumbnails, and
[07:07] I use my face a lot in thumbnails, and sometimes they get distorted and low
[07:09] sometimes they get distorted and low quality. That's why I'm actually using
[07:11] quality. That's why I'm actually using this for my own use case. Now, by the
[07:13] this for my own use case. Now, by the way, if you are finding this video
[07:14] way, if you are finding this video valuable, then I highly recommend that
[07:15] valuable, then I highly recommend that you just scroll down a little bit and
[07:17] you just scroll down a little bit and check if you're subscribed, because 90%
[07:19] check if you're subscribed, because 90% of you guys are not subscribed, and this
[07:21] of you guys are not subscribed, and this is a totally free way to show your
[07:22] is a totally free way to show your support to the channel and to also get
[07:24] support to the channel and to also get more videos just like this one in your
[07:26] more videos just like this one in your for you page. So, if you did subscribe,
[07:28] for you page. So, if you did subscribe, thank you very much. Now, once you send
[07:29] thank you very much. Now, once you send off the prompt I just showed you a
[07:30] off the prompt I just showed you a minute ago, you can type into the
[07:31] minute ago, you can type into the CloudCode something like check what AI
[07:33] CloudCode something like check what AI models I have installed locally, if you
[07:35] models I have installed locally, if you have any AI models. I'll show you how to
[07:36] have any AI models. I'll show you how to do that in just a second. Just to test
[07:38] do that in just a second. Just to test if you have the connection made. Now,
[07:39] if you have the connection made. Now, for context, these are the models that I
[07:41] for context, these are the models that I have installed. Flux 2 is really good,
[07:43] have installed. Flux 2 is really good, and the reason it's also really good is
[07:44] and the reason it's also really good is because it's open source. You can
[07:46] because it's open source. You can generate all of these images completely
[07:48] generate all of these images completely for free. These are all generated by
[07:50] for free. These are all generated by Flux 2. And if you don't have any image
[07:52] Flux 2. And if you don't have any image model installed on your computer, I'm
[07:54] model installed on your computer, I'm about to show you how to do that right
[07:55] about to show you how to do that right now. Firstly, we can open up Hugging
[07:57] now. Firstly, we can open up Hugging Face, which is like a store for these AI
[07:59] Face, which is like a store for these AI image models. And once you're on Hugging
[08:00] image models. And once you're on Hugging Face, and again, I'll leave a link to
[08:02] Face, and again, I'll leave a link to the exact model I'm going to use below,
[08:03] the exact model I'm going to use below, but you can just click down here on the
[08:04] but you can just click down here on the download button, which will install Koh
[08:06] download button, which will install Koh Image_ComfyUI.
[08:08] Image_ComfyUI. It's basically a free and open-source
[08:10] It's basically a free and open-source image model. Now, this is 19 GB in size,
[08:12] image model. Now, this is 19 GB in size, so this might take a while. But once you
[08:14] so this might take a while. But once you install it, you can basically tell
[08:15] install it, you can basically tell CloudCode that you just installed a
[08:16] CloudCode that you just installed a model, can it move that model to the
[08:20] model, can it move that model to the correct position inside of your Mac. So,
[08:22] correct position inside of your Mac. So, this way, you don't even need to know
[08:23] this way, you don't even need to know exactly where to install it to. You can
[08:25] exactly where to install it to. You can just download it into downloads, and
[08:27] just download it into downloads, and then tell CloudCode to move it into the
[08:28] then tell CloudCode to move it into the right place, move it into the right
[08:30] right place, move it into the right folder, so that you can use it. So, once
[08:32] folder, so that you can use it. So, once you have your AI image model downloaded,
[08:34] you have your AI image model downloaded, I've prepared this prompt for you to
[08:35] I've prepared this prompt for you to just copy. Again, top link in the
[08:36] just copy. Again, top link in the description. And the only things you
[08:38] description. And the only things you need to change here are the three things
[08:39] need to change here are the three things in this red box. If you're using the
[08:41] in this red box. If you're using the same exact model that I'm using, you
[08:43] same exact model that I'm using, you don't even need to change these things.
[08:44] don't even need to change these things. And you could also even remove this
[08:46] And you could also even remove this model that I already have on disk
[08:47] model that I already have on disk section. So, let me just copy this
[08:49] section. So, let me just copy this first, and then paste it into CloudCode
[08:51] first, and then paste it into CloudCode so that I can start working while I
[08:52] so that I can start working while I explain the prompt. Basically, I want to
[08:54] explain the prompt. Basically, I want to do everything inside of ComfyUI. I don't
[08:56] do everything inside of ComfyUI. I don't want to use the file.ai website, which
[08:58] want to use the file.ai website, which I'll get into in just a second. The goal
[08:59] I'll get into in just a second. The goal is to train custom LoRAs on the Koh
[09:02] is to train custom LoRAs on the Koh Image model, and then generate images
[09:03] Image model, and then generate images with them locally on my Mac. Then, I'm
[09:05] with them locally on my Mac. Then, I'm just telling it to set things up so that
[09:07] just telling it to set things up so that ComfyUI can train on a LoRA on Koh. Some
[09:10] ComfyUI can train on a LoRA on Koh. Some description of what I need. Make the
[09:11] description of what I need. Make the training workflow universal so that I
[09:13] training workflow universal so that I can train a face, style, or an object
[09:15] can train a face, style, or an object without rebuilding it, and then generate
[09:17] without rebuilding it, and then generate images with the trained LoRA locally on
[09:19] images with the trained LoRA locally on my Mac. Basically, handle the whole
[09:21] my Mac. Basically, handle the whole thing. So, while this is generating, I
[09:22] thing. So, while this is generating, I want to go to file.ai, which is
[09:25] want to go to file.ai, which is basically where we can develop and
[09:26] basically where we can develop and fine-tune models with serverless GPUs.
[09:29] fine-tune models with serverless GPUs. So, we can use their computers to
[09:30] So, we can use their computers to actually create our own LoRA. Now, what
[09:32] actually create our own LoRA. Now, what is a LoRA, because I haven't really
[09:34] is a LoRA, because I haven't really explained this yet. So, in simple terms,
[09:36] explained this yet. So, in simple terms, it's basically a small add-on to a large
[09:38] it's basically a small add-on to a large AI model that teaches it one specific
[09:41] AI model that teaches it one specific thing without touching the actual model
[09:43] thing without touching the actual model itself. So when you add this to a model,
[09:45] itself. So when you add this to a model, it'll know exactly what that specific
[09:47] it'll know exactly what that specific thing looks like. So let's say we give
[09:48] thing looks like. So let's say we give it a data set of 50 different images of
[09:50] it a data set of 50 different images of your face, it will know what your face
[09:53] your face, it will know what your face looks like so that it doesn't have to
[09:54] looks like so that it doesn't have to distort the image and guess when you're
[09:56] distort the image and guess when you're when you're changing facial positions,
[09:58] when you're changing facial positions, lighting, style, clothes, etc. Because
[10:00] lighting, style, clothes, etc. Because inside of a LoRA, there are billions of
[10:02] inside of a LoRA, there are billions of numbers which are called weights. I
[10:04] numbers which are called weights. I mean, this is the same for regular
[10:06] mean, this is the same for regular neural networks, but those weights
[10:07] neural networks, but those weights basically tell the model how the
[10:08] basically tell the model how the specific thing will look like. And
[10:10] specific thing will look like. And obviously, these LoRAs are created
[10:11] obviously, these LoRAs are created during LoRA training, which we'll do on
[10:13] during LoRA training, which we'll do on file.io website in just a second. And
[10:16] file.io website in just a second. And the only thing we need to train is the
[10:17] the only thing we need to train is the actual data set. So what is that you
[10:20] actual data set. So what is that you want to train on? Are you trying to
[10:21] want to train on? Are you trying to train some sort of a product or some
[10:24] train some sort of a product or some sort of a face or a style? The more data
[10:26] sort of a face or a style? The more data you have, the better your training is
[10:28] you have, the better your training is going to turn out. And also, the same
[10:30] going to turn out. And also, the same goes for the more high-quality data you
[10:31] goes for the more high-quality data you have. So if you're taking pictures of a
[10:33] have. So if you're taking pictures of a product, make sure to take as many and
[10:36] product, make sure to take as many and as many high-quality images of that
[10:37] as many high-quality images of that product as possible. So then we can add
[10:39] product as possible. So then we can add a trigger word which helps the model
[10:41] a trigger word which helps the model understand what it should learn, like a
[10:42] understand what it should learn, like a man, a style, a cop, etc. You set up
[10:45] man, a style, a cop, etc. You set up some parameters like how many steps it
[10:46] some parameters like how many steps it should do to learn, and you can just
[10:48] should do to learn, and you can just simply ask Claude how many steps for
[10:50] simply ask Claude how many steps for your specific use case you need, but
[10:52] your specific use case you need, but I'll get into more of this in just a
[10:53] I'll get into more of this in just a moment. Now the inside process looks
[10:55] moment. Now the inside process looks kind of like this. The LoRA adds a tiny
[10:57] kind of like this. The LoRA adds a tiny number of weights to the diffusion model
[10:59] number of weights to the diffusion model and trains these until the modified
[11:01] and trains these until the modified model understands the concept. So
[11:03] model understands the concept. So basically, it adds noise to the image
[11:05] basically, it adds noise to the image and then asks the model with the LoRA to
[11:07] and then asks the model with the LoRA to guess what noise was added. Based on
[11:09] guess what noise was added. Based on response, it knows how wrong it was and
[11:11] response, it knows how wrong it was and then it adjusts its answers over time to
[11:14] then it adjusts its answers over time to make better and better predictions. Now
[11:15] make better and better predictions. Now after that, it sends the corrections to
[11:17] after that, it sends the corrections to change the LoRA by a tiny step until
[11:19] change the LoRA by a tiny step until eventually it basically has as little
[11:21] eventually it basically has as little error loss as possible. And when
[11:22] error loss as possible. And when everything is done, you get the safe
[11:23] everything is done, you get the safe tensors file, which is your trained
[11:26] tensors file, which is your trained LoRA. This is just a bunch of different
[11:28] LoRA. This is just a bunch of different numbers or a bunch of different weights,
[11:29] numbers or a bunch of different weights, which we spoke about earlier. And these
[11:30] which we spoke about earlier. And these safe tensors is basically what you apply
[11:32] safe tensors is basically what you apply to the image model. And as you can see,
[11:34] to the image model. And as you can see, it's cloud just finished. It was taking
[11:36] it's cloud just finished. It was taking It basically took 6 and 1/2 minutes for
[11:38] It basically took 6 and 1/2 minutes for this to finish. Okay, it's not done yet.
[11:39] this to finish. Okay, it's not done yet. Now it's actually building the first
[11:40] Now it's actually building the first node. But we're going to let it cook
[11:42] node. But we're going to let it cook because as I said, we don't need to do
[11:43] because as I said, we don't need to do much with the actual workflow process
[11:45] much with the actual workflow process here. What we do need to do is go to the
[11:47] here. What we do need to do is go to the file.io website and we need to get our
[11:49] file.io website and we need to get our own API key. An API key is basically an
[11:52] own API key. An API key is basically an authentication method. It just allows
[11:53] authentication method. It just allows you to connect to external services. So
[11:56] you to connect to external services. So for this use case, we're going to use it
[11:58] for this use case, we're going to use it to connect our cloud code instance to
[12:00] to connect our cloud code instance to file.io's website. Once you're at the
[12:02] file.io's website. Once you're at the file.io website, you can go up here to
[12:04] file.io website, you can go up here to credits, go to API keys, and simply add
[12:06] credits, go to API keys, and simply add a new key. You can add a description,
[12:08] a new key. You can add a description, and then just click on create key. And
[12:09] and then just click on create key. And this is basically your password to your
[12:11] this is basically your password to your file website, which means anyone that
[12:13] file website, which means anyone that has this key can take your credits, can
[12:15] has this key can take your credits, can use your account. So don't share this
[12:17] use your account. So don't share this with anyone. But once you have this API
[12:18] with anyone. But once you have this API key, copy it. Now once you have it
[12:20] key, copy it. Now once you have it copied, we can go back to cloud code and
[12:21] copied, we can go back to cloud code and we can see that everything is built,
[12:23] we can see that everything is built, verified, and the generation workflow is
[12:24] verified, and the generation workflow is valid against the live server. So this
[12:26] valid against the live server. So this is what we need to do. We need to Step
[12:28] is what we need to do. We need to Step one, paste your file AI key in this
[12:30] one, paste your file AI key in this file. So rather than looking for the
[12:31] file. So rather than looking for the file, I'm going to say, "Open the file
[12:32] file, I'm going to say, "Open the file for me where I need to paste my API
[12:34] for me where I need to paste my API key." And as you can see, it opened up
[12:36] key." And as you can see, it opened up this .txt file. So I'm just going to
[12:37] this .txt file. So I'm just going to paste my key in here. Okay, I replaced
[12:39] paste my key in here. Okay, I replaced my key and I'm just going to say done.
[12:40] my key and I'm just going to say done. Okay, basically I stopped recording and
[12:41] Okay, basically I stopped recording and I forgot to continue recording. So I
[12:43] I forgot to continue recording. So I already done a couple of steps here, so
[12:44] already done a couple of steps here, so let me explain what I done. We just
[12:45] let me explain what I done. We just added our API key into the text file. So
[12:47] added our API key into the text file. So now the key is saved correctly and I
[12:49] now the key is saved correctly and I have more instructions. What we need to
[12:51] have more instructions. What we need to do now is quit Comfy Desktop completely
[12:54] do now is quit Comfy Desktop completely and reopen it. So obviously just quit
[12:55] and reopen it. So obviously just quit and reopen. Then click on workflows, and
[12:57] and reopen. Then click on workflows, and as you can see, we have two new
[12:58] as you can see, we have two new workflows ready to go. Train and
[13:00] workflows ready to go. Train and generate. We're going to click on train,
[13:02] generate. We're going to click on train, and we're going to click on workflows
[13:03] and we're going to click on workflows once again. And now we can see that we
[13:04] once again. And now we can see that we have a readme file right here. This is
[13:06] have a readme file right here. This is basically instructions for you the human
[13:08] basically instructions for you the human to follow along. So what I did is I
[13:10] to follow along. So what I did is I clicked shift command four, took a
[13:12] clicked shift command four, took a screenshot of this, pasted the
[13:13] screenshot of this, pasted the screenshot into the terminal, and simply
[13:15] screenshot into the terminal, and simply said, "Open the file where you want me
[13:17] said, "Open the file where you want me to add the images into." Because if you
[13:19] to add the images into." Because if you read the steps, the step one is put your
[13:21] read the steps, the step one is put your file key into this file, which we
[13:22] file key into this file, which we already done. Step two is put 10 to 25
[13:25] already done. Step two is put 10 to 25 images into a folder, and then it gives
[13:27] images into a folder, and then it gives you the folder directory. Well, I cannot
[13:29] you the folder directory. Well, I cannot be asked looking for this, so I just
[13:31] be asked looking for this, so I just told Cloud Code to open it for me and it
[13:32] told Cloud Code to open it for me and it done just that. It opened this folder
[13:34] done just that. It opened this folder for me and then I just dragged all of my
[13:36] for me and then I just dragged all of my images, all of my data into this folder.
[13:38] images, all of my data into this folder. So, any process that you are struggling
[13:40] So, any process that you are struggling with throughout this video, tell Cloud
[13:42] with throughout this video, tell Cloud Code to help you, give it exact,
[13:44] Code to help you, give it exact, detailed instructions and explain to it
[13:46] detailed instructions and explain to it what you're struggling with. There's no
[13:47] what you're struggling with. There's no excuses. If I'm going too fast, just
[13:50] excuses. If I'm going too fast, just consult with Cloud Code about what it is
[13:52] consult with Cloud Code about what it is that you want to change or what it is
[13:54] that you want to change or what it is that you are struggling with. Anyways,
[13:55] that you are struggling with. Anyways, the next step in this read me is to set
[13:58] the next step in this read me is to set images folder, so we're going to do
[13:59] images folder, so we're going to do that. So, I'm just going to leave it as
[14:01] that. So, I'm just going to leave it as is right here. Trigger word, I'll just
[14:02] is right here. Trigger word, I'll just do face. Subject type, we have three
[14:05] do face. Subject type, we have three different types. We have face, person,
[14:06] different types. We have face, person, style and object. So, I have face
[14:09] style and object. So, I have face person. So then going back into ComfyUI,
[14:11] person. So then going back into ComfyUI, for the steps, you can just ask Cloud
[14:13] for the steps, you can just ask Cloud Code how many steps should I do? Same
[14:15] Code how many steps should I do? Same thing with learning rate. Okay, so this
[14:16] thing with learning rate. Okay, so this is actually a good example cuz right now
[14:18] is actually a good example cuz right now it's saying I need to queue it's start
[14:19] it's saying I need to queue it's start training off. When I checked this, it
[14:21] training off. When I checked this, it also said leave start training off and
[14:23] also said leave start training off and click queue. But there is no queue. So,
[14:26] click queue. But there is no queue. So, I have a question, so I'm going to
[14:27] I have a question, so I'm going to basically screenshot the screen and then
[14:28] basically screenshot the screen and then I just pasted the screenshot into my
[14:30] I just pasted the screenshot into my terminal into the Cloud Code and I'm
[14:31] terminal into the Cloud Code and I'm going to say there is no queue button.
[14:33] going to say there is no queue button. Do I just click on run? Does everything
[14:35] Do I just click on run? Does everything look correct? Double check. And as you
[14:37] look correct? Double check. And as you can see, I got my answer. So, I want to
[14:39] can see, I got my answer. So, I want to keep this in because I want to show you
[14:40] keep this in because I want to show you anything that you're struggling with,
[14:41] anything that you're struggling with, you can just ask Cloud Code. This was a
[14:43] you can just ask Cloud Code. This was a perfect use case for it. There was no
[14:45] perfect use case for it. There was no queue button, but I presumed it was the
[14:46] queue button, but I presumed it was the run button, but I wasn't sure. So,
[14:47] run button, but I wasn't sure. So, that's why I asked. So again, it said
[14:49] that's why I asked. So again, it said start training dry run, no changes. If
[14:51] start training dry run, no changes. If you click on this, you can change now,
[14:52] you click on this, you can change now, but let's just do a dry run. So, let's
[14:54] but let's just do a dry run. So, let's click on run. And so now we just did
[14:56] click on run. And so now we just did that. It run and now to see result, open
[14:58] that. It run and now to see result, open the console on the left side bar near
[15:00] the console on the left side bar near the bottom. Okay, so as you can see, we
[15:01] the bottom. Okay, so as you can see, we have the estimated cost, which is going
[15:03] have the estimated cost, which is going to be about $1.50. So, sorry for lying
[15:05] to be about $1.50. So, sorry for lying earlier cuz I said $2, but it's actually
[15:07] earlier cuz I said $2, but it's actually $1.50 and we can see the amount of
[15:09] $1.50 and we can see the amount of images was correct cuz I have 35 images
[15:11] images was correct cuz I have 35 images in that folder. So, everything is
[15:13] in that folder. So, everything is correct. Well, okay, in that case, we
[15:14] correct. Well, okay, in that case, we can, I guess, change the start training
[15:16] can, I guess, change the start training to train now and we can just click on
[15:17] to train now and we can just click on run. And this is only going to take a
[15:19] run. And this is only going to take a few minutes cuz this isn't running on my
[15:21] few minutes cuz this isn't running on my machine right now. This is connected
[15:23] machine right now. This is connected once again to file.ai via the API key
[15:26] once again to file.ai via the API key that we pasted in earlier. And as you
[15:28] that we pasted in earlier. And as you can see, it even says cloud right here.
[15:30] can see, it even says cloud right here. So, you shouldn't worry too much about
[15:32] So, you shouldn't worry too much about the costs because firstly, you will need
[15:34] the costs because firstly, you will need to have a certain amount of credits
[15:35] to have a certain amount of credits topped up. So, unless you have auto
[15:37] topped up. So, unless you have auto top-up enabled, it's not going to just
[15:39] top-up enabled, it's not going to just drain your wallet. It might drain your
[15:40] drain your wallet. It might drain your credits if you topped up, I don't know,
[15:41] credits if you topped up, I don't know, $1,000 in credits. But even at that, we
[15:43] $1,000 in credits. But even at that, we just have the estimated price. So, there
[15:45] just have the estimated price. So, there is no need to worry about how much is
[15:47] is no need to worry about how much is actually going to cost. Now, what I'm
[15:48] actually going to cost. Now, what I'm going to do is send another screenshot
[15:50] going to do is send another screenshot and just ask how long does this process
[15:53] and just ask how long does this process take cuz I'm curious how long am I going
[15:54] take cuz I'm curious how long am I going to be waiting here. I'm also going to
[15:56] to be waiting here. I'm also going to ask, "What is the status? Can you see
[15:58] ask, "What is the status? Can you see the status?" Okay, nice. So, I can see
[16:00] the status?" Okay, nice. So, I can see it's live pulled from comfyui log. So,
[16:02] it's live pulled from comfyui log. So, for some reason this is saying 0% but
[16:03] for some reason this is saying 0% but it's saying it is about 21% done. So,
[16:06] it's saying it is about 21% done. So, yeah, about 10 minutes remaining. But
[16:07] yeah, about 10 minutes remaining. But again, if you were to do this locally, I
[16:09] again, if you were to do this locally, I mean, if you have a regular PC, it would
[16:11] mean, if you have a regular PC, it would probably be somewhere in the range of 6
[16:13] probably be somewhere in the range of 6 hours maybe. I'm just estimating here, I
[16:15] hours maybe. I'm just estimating here, I don't know. But doing this on cloud for
[16:16] don't know. But doing this on cloud for 10 to 20 minutes for $2 is definitely
[16:18] 10 to 20 minutes for $2 is definitely worth it. That's why I'm showing you
[16:20] worth it. That's why I'm showing you this method. If you want to do this
[16:21] this method. If you want to do this locally, you can do this locally as well
[16:23] locally, you can do this locally as well if you have your own rig. But I'm just
[16:24] if you have your own rig. But I'm just going to let it cook and I'll come back
[16:25] going to let it cook and I'll come back to it when it's ready. And as you can
[16:27] to it when it's ready. And as you can see, it's saying that it's completely
[16:28] see, it's saying that it's completely done, 1,000 out of 1,000 steps. So, the
[16:31] done, 1,000 out of 1,000 steps. So, the next step is to generate the Laura. So,
[16:33] next step is to generate the Laura. So, what we need to do is go back to
[16:34] what we need to do is go back to workflows and open the generate section.
[16:37] workflows and open the generate section. So, we're going to click on workflows,
[16:38] So, we're going to click on workflows, double tap on generate, press R just to
[16:40] double tap on generate, press R just to refresh. And by the way, I didn't do any
[16:42] refresh. And by the way, I didn't do any of this workflow. Again, this workflow
[16:43] of this workflow. Again, this workflow was entirely designed by Cloud Code. I'm
[16:45] was entirely designed by Cloud Code. I'm going to just separate these a little
[16:46] going to just separate these a little bit. So, I wasn't over exaggerating when
[16:48] bit. So, I wasn't over exaggerating when I said that you can just create all the
[16:49] I said that you can just create all the workflows for you. And as you can see,
[16:51] workflows for you. And as you can see, I'm also just following what Cloud Code
[16:53] I'm also just following what Cloud Code is telling me to do. I didn't script
[16:55] is telling me to do. I didn't script this part of it. I'm just doing it in
[16:56] this part of it. I'm just doing it in real time. So, we need to go to your
[16:58] real time. So, we need to go to your Laura A node, click on the drop down,
[17:00] Laura A node, click on the drop down, pick the my coin Laura save tensors. So,
[17:03] pick the my coin Laura save tensors. So, this is your Laura A. Click on the drop
[17:05] this is your Laura A. Click on the drop down and my coin Laura save tensors. Set
[17:08] down and my coin Laura save tensors. Set its strength to one to turn it on. So,
[17:10] its strength to one to turn it on. So, let's do one. On the prompt edit me,
[17:12] let's do one. On the prompt edit me, write a prompt that includes your
[17:14] write a prompt that includes your trigger word, face. Okay, so this is the
[17:16] trigger word, face. Okay, so this is the trigger word that we added earlier. So,
[17:18] trigger word that we added earlier. So, what I'm going to say is face wearing a
[17:20] what I'm going to say is face wearing a space suit. Super simple prompt. Leave
[17:23] space suit. Super simple prompt. Leave the lighting at one, keeps it fast, and
[17:25] the lighting at one, keeps it fast, and I think that should be it. So, let's
[17:27] I think that should be it. So, let's basically double-check. I mean, it looks
[17:29] basically double-check. I mean, it looks good. Let's just click on run. So, now
[17:31] good. Let's just click on run. So, now it is actually running manually on my
[17:34] it is actually running manually on my machine, so we can see which node is
[17:36] machine, so we can see which node is actually being worked on in real time.
[17:37] actually being worked on in real time. But, it's also saying that maybe causing
[17:39] But, it's also saying that maybe causing problems since it's a very generic word.
[17:42] problems since it's a very generic word. So, if this is causing problems, we can
[17:44] So, if this is causing problems, we can retrain it with a more rare token name
[17:46] retrain it with a more rare token name like mache or with a four, which fixes
[17:48] like mache or with a four, which fixes it cleanly because no one's ever going
[17:50] it cleanly because no one's ever going to say this in their regular prompt.
[17:51] to say this in their regular prompt. But, face is a bit more common, so I
[17:53] But, face is a bit more common, so I guess that's a tip that you can take
[17:54] guess that's a tip that you can take with you that I didn't personally do,
[17:56] with you that I didn't personally do, but we'll see what it turns out like.
[17:57] but we'll see what it turns out like. While I'm waiting, I'm just going to say
[17:58] While I'm waiting, I'm just going to say open a folder where these saved images
[18:00] open a folder where these saved images are going to be located. Okay? As you
[18:03] are going to be located. Okay? As you can see, it opened up this folder, so I
[18:04] can see, it opened up this folder, so I presume that this is where the images
[18:06] presume that this is where the images are going to be located. And so, we have
[18:07] are going to be located. And so, we have our result. This is me in a space suit.
[18:09] our result. This is me in a space suit. It doesn't really look like a space
[18:10] It doesn't really look like a space suit, I'll be honest, but this isn't too
[18:12] suit, I'll be honest, but this isn't too much of the fault of the Laura. This is
[18:14] much of the fault of the Laura. This is more a fault of the model that we used.
[18:16] more a fault of the model that we used. So, the biggest problem right now is
[18:18] So, the biggest problem right now is just the fact that I don't have a GPU
[18:19] just the fact that I don't have a GPU cluster, I guess. So, I do personally
[18:21] cluster, I guess. So, I do personally think that running this on comfy cloud
[18:23] think that running this on comfy cloud is a better option because we'll be able
[18:25] is a better option because we'll be able to actually use more powerful PCs and
[18:27] to actually use more powerful PCs and actually get better quality images, but
[18:29] actually get better quality images, but the face looks good. This face does look
[18:30] the face looks good. This face does look like me. And again, if I wanted to
[18:31] like me. And again, if I wanted to regenerate this, it already has all the
[18:33] regenerate this, it already has all the context, and all I would need to do is
[18:35] context, and all I would need to do is just change one prompt. I don't need to
[18:37] just change one prompt. I don't need to repeat this over and over again. The
[18:38] repeat this over and over again. The workflow is created, the Laura is
[18:40] workflow is created, the Laura is created, and now you know how to do
[18:41] created, and now you know how to do this. And we can do this again and again
[18:43] this. And we can do this again and again for different products, styles, people.
[18:44] for different products, styles, people. So again, we did not even need to touch
[18:46] So again, we did not even need to touch any of these nodes because cloud code
[18:49] any of these nodes because cloud code just simply don't order that for us.
[18:50] just simply don't order that for us. Now, if you want to see how cloud code
[18:51] Now, if you want to see how cloud code can also edit videos and create motion
[18:54] can also edit videos and create motion graphics, not like this one, this is the
[18:55] graphics, not like this one, this is the old one, kind of like this one, the one
[18:57] old one, kind of like this one, the one you're seeing on screen right now, then
[18:58] you're seeing on screen right now, then just click on the video that's going to
[18:59] just click on the video that's going to be somewhere on screen right now, and
[19:00] be somewhere on screen right now, and I'll see you there.

# How Modders Unlocked a $5,000 Chip Inside Normal Nvidia GPUs

> Auto-generated from video transcripts.

---

### How Modders Unlocked a $5,000 Chip Inside Normal Nvidia GPUs

- **Source:** https://youtu.be/nRj0iRSBMwc?is=GukHf9hZUEvlIUTP
- **Duration:** 08:18 (498.1s)

### Key Points

- **[00:00]** On October 24th, an open-source repository called&nbsp; gpu-unlock dropped on GitHub with a single,&nbsp;&nbsp;
- **[00:15]** For years, Nvidia has taken the exact&nbsp; same silicon chip used inside its&nbsp;&nbsp;
- **[00:26]** and secretly throttled half of its&nbsp; high-precision AI math performance in firmware.&nbsp;
- **[00:37]** hidden hardware switches milliseconds before&nbsp; your PC boots—doubling peak Tensor instruction&nbsp;&nbsp;
- **[00:51]** But when testers pushed these unlocked&nbsp; cards to the limit days later,&nbsp;&nbsp;
- **[01:06]** To see why this mod is sending shockwaves through&nbsp; the PC hardware world, look at how graphics&nbsp;&nbsp;
- **[01:18]** Therefore, Nvidia doesn't build a brand-new&nbsp; chip from scratch for its five-thousand-dollar&nbsp;&nbsp;
- **[01:29]** professional RTX A6000 share the exact&nbsp; same underlying GA102 silicon die.&nbsp;&nbsp;
- **[01:43]** Cores—specialized blocks of hardware engineered&nbsp; for the heavy matrix math behind local AI models,&nbsp;&nbsp;
- **[01:55]** Tensor calculations at full speed, startups and&nbsp; researchers would simply buy cheap gaming GPUs&nbsp;&nbsp;
- **[02:08]** 30, and 40 series, Nvidia placed an&nbsp; artificial speed limit inside the firmware.&nbsp;&nbsp;
- **[02:21]** chip intentionally cuts its own instruction rate&nbsp; in half. Everyone assumed that cap was permanently&nbsp;&nbsp;
- **[02:39]** Historically, trying to bypass an&nbsp; Nvidia hardware lock was a nightmare.&nbsp;&nbsp;
- **[02:50]** rejected the signature and bricked the display.&nbsp; And if you patched the Nvidia driver file,&nbsp;&nbsp;
- **[03:03]** timing trick. Instead of touching the BIOS or&nbsp; hacking Nvidia’s driver, the tool installs as&nbsp;&nbsp;
- **[03:15]** before Nvidia’s official driver even loads. In that tiny fraction of a second during startup,&nbsp;&nbsp;
- **[03:27]** and rewrites two hidden hardware speed-selector&nbsp; registers—flipping selector SS0 from a&nbsp;&nbsp;
- **[03:43]** Immediately after flipping the switch,&nbsp; the module triggers a quick PCIe Function&nbsp;&nbsp;
- **[03:54]** A moment later, Nvidia’s unmodified&nbsp; official driver wakes up, reads the card,&nbsp;&nbsp;
- **[04:11]** Within seventy-two hours of Zagitta&nbsp; releasing the code for RTX 30-series cards,&nbsp;&nbsp;
- **[04:22]** called gpu-unlock-ada to unlock the RTX 4090 and&nbsp; experimental 40-series desktop and laptop cards,&nbsp;&nbsp;
- **[04:38]** When the first community benchmarks rolled&nbsp; in across Tom’s Hardware and TechSpot on&nbsp;&nbsp;
- **[04:50]** four-hundred-and-fifty-watt limit, BF16 matrix&nbsp; throughput surged from eighty-seven point five to&nbsp;&nbsp;
- **[05:01]** exact same hardware. Meanwhile, TF32 compute&nbsp; nearly doubled, climbing ninety-four percent.&nbsp;
- **[05:15]** inside ComfyUI, thirty-three to fifty-five&nbsp; percent faster prompt processing in local&nbsp;&nbsp;
- **[05:25]** with bit-for-bit identical output accuracy.&nbsp; Even wilder, a separate developer proved the&nbsp;&nbsp;
- **[05:38]** But when testers unlocked the flagship&nbsp; RTX 4090, something unexpected happened.
- **[06:00]** one hundred and sixty-five teraflops to an insane&nbsp; three hundred and thirty-one point six teraflops.&nbsp;&nbsp;
- **[06:12]** the actual real-world speedup was&nbsp; only twelve to thirty-four percent.&nbsp;
- **[06:25]** that the graphics card literally runs&nbsp; out of electrical power before it runs&nbsp;&nbsp;
- **[06:34]** and downclocking itself to survive. At the same time, testing revealed&nbsp;&nbsp;
- **[06:45]** than high-precision AI Tensor math, your&nbsp; gaming framerate stays completely identical.&nbsp;&nbsp;
- **[06:56]** if an RTX laptop ever puts its graphics&nbsp; card into deep sleep to save battery,&nbsp;&nbsp;
- **[07:12]** Even with those limits, gpu-unlock has exposed one&nbsp; of the dirtiest realities in modern technology:&nbsp;&nbsp;
- **[07:26]** only to have half of its capability locked&nbsp; behind two software digits in a startup file.&nbsp;
- **[07:39]** working unlock has been found for Nvidia’s&nbsp; newest RTX 50-series architectures—suggesting&nbsp;&nbsp;
- **[07:50]** where pre-boot scripts can never reach them. For now, hundreds of thousands of older RTX&nbsp;&nbsp;
- **[08:03]** AI to turn PlayStation 5 games into native PC&nbsp; apps, or how a viral GitHub tool is ripping&nbsp;&nbsp;

---

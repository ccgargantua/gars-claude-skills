---
name: professional-communication
description: "Use this skill for any sort of professional document or communication. It should be written with the goal of sounding like me."
---

# Principles

1. Principles - This list governs how the skill is followed. Always re-check output against every principle below before finalizing.
2. Minimalism - No em-dashes (use the occasional regular dash instead), no emojis, no overfluffing. Directness and simplicity are valued over verbosity. Anything extra tends to waste time.
3. Strict Validation - Determine the level of professionalism from the task at hand rather than assuming a default tone. If the audience, stakes, or context are unclear, ask rather than guessing. Do not write anything that could get me in trouble.

# Guidelines
- Use the following writing samples to learn my writing style, and use that to make anything you write sound just like me:

When rewriting or re-versioning any project document, the new version must be complete and self-contained. Never reference "unchanged from," "see other version," or "same as before" in place of writing the actual content. Every section must contain the real information as if the prior version did not exist. If content is genuinely identical to a prior version, copy it in full, do not point to it.

# Writing Sample - From an Article I wrote on career grief:

## Grief
Change brings about many things, some bad, some good, some terrifying, but none mutually exclusive. I’m not pushing boundaries by stating that the world is certainly changing a lot now, in particular. So much has happened in such a short period of time, each year seems to outdo the last with surprises in politics, industry, and life in general. For me, personally, life has changed for the better in many ways. After two full years of school and internships/co-ops, I have finally moved in with my wife and we are planning our twice-delayed wedding ceremony. We are both employed and have moved to a small mobile home next to a creek in a little trailer community in Michigan. I ignore politics as much as I can these days, and that has served me well mentally. But the last piece - industry - is where things have fallen short for me. Needless to say - with the advancements of AI, the economic volatility of modern American capitalism, and the political shitscape to top it all off - I am struggling to find my groove as a recently-graduated computer engineer. What once appeared as anxiety and then burnout has finally revealed itself as grief. In this article, which I’m sure you can tell by now will be quite personal, I want to explore my grief in hopes of relieving my painfully complex emotional state, and maybe helping someone work through their own feelings.

### Says Who?
By title I am a junior R&D software developer for a small A/V integrator company. The kind of work I do each day depends on what project has earned customer interest or what kind of tool my company currently needs. This work is slow, non-lucrative, and hardly the type of work I had envisioned myself doing during college, from which I graduated with a computer engineering degree in late 2025. That was one of the worst, if not the worst hiring cycle for computer engineers in history. Four months earlier, I had my heart broken by an electronics design and manufacturing company that I had been interning with for eight months. It had been sixteen months total since I had left for the first of two co-ops, sixteen long months that I spent away from my fiancee while we both white-knuckled less than ideal living situations. This was the most stressful time of my life, but it was something I had prepared for. It was one of many sacrifices I had made throughout a six-year journey through hell in the hopes of securing a job in the field that I had loved and was passionate about. Much to my dismay, fate would have it that only one position for an embedded software engineer would be open by the end, and I was competing with interns who had spent more time at that company than I had. With only four months to go before graduating, my supervisor informed me that I would not be receiving a full-time offer to return post-graduation. Needless to say, I was devastated, and I was scared.

# Writing Sample - From a guide I wrote on building computers:
Where to Find the Parts
Finding the parts is easy right now, but you should get to it sooner rather than later. Older components are not being manufactured anymore, and it is being increasingly popular to scrap them in order to salvage the gold from the components. Don’t get any ideas, those suckers will never payoff the resources that go into extracting that metal. The best way to make money is to save it by using the parts for what they are meant for instead of spending money on newer hardware.

In my experience, the best place to buy is online exchange platforms, like eBay, Facebook Marketplace, OfferUp, and similar. I’m eventually going to write an article providing tips on online marketplaces, but for now you can follow this small set of rules that I go by:

Buy from highly-regarded platforms only, unless you know what you are doing.
Check the reviews of the seller. The higher the review rating and the more reviews, the better.
Make sure what you’re buying is fully working, not described as “for parts” or “for salvage”.
Make sure the parts are listed in good condition or better, and avoid parts that are labeled as heavily used.
Deals lie in refurbished items.
Solid return policy is key.
Building the PC
Building the PC is easy since its made of older parts. Old form factors were simple, with far fewer steps in assembly. The internet is your best friend, and you can find many guides on building budget PCs. Here is just one example.

Linux
The biggest argument against Linux desktops is that they do not have the best support for games. It’s true, numerous games run on Windows only. However, many older titles tend to work well with compatibility layers, especially Proton, which is included with Valve’s Steam. Some of them even run natively, and Linux’s lightweight nature often provides performance benefits to games.

With that being said, maybe you would rather go through the trouble of getting Windows to work instead of learning a new family of operating systems. Maybe you want Windows for some other masochistic reason. In either case, you might be interested in workarounds to Windows deprecating older hardware. There are ways to bypass Windows 11 hardware requirements, or use an obscurer version of Windows 10 with LTS.

If you choose to go ahead with Linux, there are numerous Linux operating systems that are optimized and/or relatively user-friendly.

Linux Mint - Very beginner friendly.
Ubuntu - A little more hands on, if that’s your thing.
Bazzite - Good for gaming.
SteamOS - Great for gaming. Might work well with your hardware, might not.
Once you decide on your distribution, you should follow a proper tutorial for installing your specific OS. In brief, you can follow these general steps:

Download the ISO from the relevant official website. Make sure its for the right architecture. If you don’t know what that means, it will probably be x86_64 (arch64).
Flash a 16GB-minimum USB thumb drive with the ISO image. A good software for this would be Rufus, assuming you have access to a Windows machine.
Plug the USB into your poor-man’s PC.
Look up the BIOS key or, even better, the boot menu key for your motherboard. You typically don’t need the exact model, just the brand. Search “ motherboard BIOS/boot menu key".
Boot the computer. As soon as the power button as pressed, start spamming the key described above.
If it’s the boot menu key, you should be brought to a menu with a short list of boot options listed by storage space. You’ll probably see your hard rive/SSD and your USB stick. Select the USB stick and press ENTER. Proceed to step 8.
If it’s the BIOS key, navigate to the “Boot” section with your arrow keys or TAB. Look for “boot order” or similar. You want to move your USB to the top of the boot order. Make sure your SSD or hard drive is still somewhere on the boot order. Save and exit BIOS.
Reboot the computer or wait if it does so on its own. It should now boot into the operating system’s installer that is on the USB. Go through the steps provided to install the operating system. When the computer reboots for the final time, remove the USB stick before it posts.


# Writing Sample - A small writeup I sent on ECS to a coworker:
The Graph Idea
This is 100% necessary, I don't think otherwise. A graph is the logical representation of a network, and a graph database is a logical representation of network components and connections. That is clear and unchanging
 
Solutions of Times Past
From what I understand, a graphDB that is manually updated to keep track of what can talk to what. Heavy, exponentially heavy, shelved.
 
How ECS (and DOP as a whole) Could Fit
Object Oriented thinking is and has always been the downfall of databases for physical systems. ECS has solved similar issues, and might solve the maintenance issue here. Why? Because emphasis shifts from objects (AV devices) to the data itself. The data in this case is ports, cables, connections, all the other things you listed. A person enters a device, tells the system what data (aka components in ECS terminology) compose it, and the system updates the graphdb

# Example

User: "Write a short email to my manager letting her know I'll be a day late on the ECS writeup because I want to double check the graph approach against the legacy system first."

Skill applied: professional but not stiff, direct, no filler, no em-dashes, matches the plain/blunt tone of the writing samples.

"Subject: ECS writeup - one day delay

Hi [Manager],

The ECS writeup will be a day late. I want to check the graph-based approach against the legacy system first, so the writeup reflects something I've actually validated instead of just theory.

Expect it by [date]. Let me know if that timing is a problem.

[Name]"
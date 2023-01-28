---
layout: post
title:  "Memory editing : Gaining infinite health and ammo"
date:   2023-01-28
image:  ../images/MemoryEditing/MemoryEditing.gif
excerpt: "Editing the memory of a video game to gain unfair advantage 👾"
---

# Memory editing : Gaining infinite health and ammo

## Introduction

Greetings! Welcome to my very first game hacking post. Allow me to confess that I am really excited to be writing about this topic because it is something I have always been curious about since I started playing video games. Everyone has tried to cheat on some game at least once, right? No matter whether it was about leveling up faster, becoming immortal or gaining infinite gold (shame on you if you couldn’t stop spamming `motherlode` while playing The Sims and shame on you for playing The Sims), there are games in which cheating makes the game better.

By the way, I shouldn’t need to say this but I am going to be a nice citizen and will go for a friendly reminder : **THIS IS FOR EDUCATIONAL PURPOSES ONLY**, ruining other player’s experiences is not fun at all and is often against the license of any game, so please whenever you game hack, always do so in an environment in which you are hacking for fun, not profit.

## Memory Editing

One type of tool that you will be constantly using when game hacking is a memory editor/scanner. These tools allow you to read the memory of a process and modify it at will. Do you start to see how powerful this is? Only by understanding that a game is a bunch of 0s and 1s running in your computer, you open the door to a whole new world. When you know how these 0s and 1s work, and how you can change them for your personal interest, you can achieve whatever you want in a video game.

There are many memory editors available out there (I will show you how to create a simple one in a future post), and each one has its own set of pros and cons, however, there’s a series of traits that are shared by all of them, which are :

- They allow you to see the memory of a process.
- They allow you to search and filter through that memory.
- They allow you to edit whatever you want in that memory.

### Cheat Engine

I think is not even arguable that [Cheat Engine](https://cheatengine.org/) is the most popular memory scanner that exists nowadays, and it has well earned its fame.

Yes, I know their website looks like a deep web scam, but as long as you decline all of the programs that are offered to be installed during its installation process, you are fine.

I will show you some of the tool’s basics throughout this writing, but if you want to see Cheat Engine’s full potential, you can always dig into [their Wiki](https://wiki.cheatengine.org/index.php?title=Main_Page).

What will we be using today? We are going to use the functionalities of memory scanning, filtering and editing, but in a very understandable and easy manner.

### Too much health & ammo

The game I will be using for this demonstration is [Assault Cube](https://assault.cubers.net/). It is an open source game and I highly recommend it to learn game hacking, since it has no security measures whatsoever.

To follow along this guide (which I recommend if you want to improve your skills), you will only need Assault Cube, Cheat Engine and a Windows OS. Don’t worry if you are not a Windows user, I am running this on a VM (Windows offers virtual machines for investigating purposes which you can download officially [here](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)) and it runs perfectly even with a low spec computer.

Let’s get started!

![Untitled](../images/MemoryEditing/Untitled.png)

Here you can see Cheat Engine once it is attached to the Assault Cube client. To accomplish that, you only need to click on the little top-left computer icon with a magnifying glass, then search and select the game.

There are a bunch of options that we see at first glance, but you’ll see how easy it is to change some memory values and play around a little bit.

The key concept that you will exploit when trying to scan for any value is that values change, and you can filter for values that have changed in order to find that what you want. For example, now our health is 100, so we can look for a *4 byte integer* that has the value 100 (because that is the standard way of storing a value that ranges from 0 to 100). What is the issue with that? You’ll quickly find out… **

![Untitled](../images/MemoryEditing/Untitled%201.png)

910 results, that’s the issue. Now go and brute force until you find which one is the health of your player, you may even make your enemy invincible before you find your health address!

Obviously, this won’t be the plan. As you can observe, there is a `Next Scan` button that is screaming at us, this button will filter through the 910 results to find whatever value we tell it next. Maybe we can scan again for the number 100, but there’s an even better idea. What if we take some damage on purpose? That’s it, our health will change, and the number of variables that had the value 100 and now our health will be heavily decreased.

![Untitled](../images/MemoryEditing/Untitled%202.png)

Here’s what happened, I took a bit of a suicidal move and grabbed a grenade and threw it to my foot, and then I clicked the `Next Scan` button and specified the value 38. There we go now, only 2 results, now brute forcing looks like a plan, doesn’t it?

The next step is simply double clicking the first address, and it will be moved to our saved addresses box (the big one down below) and from there, we can modify the value that the address is pointing to.

![Untitled](../images/MemoryEditing/Untitled%203.png)

Good! Now I can throw myself 20 grenades, which is what everyone plays games for.

Let’s repeat the same steps but for the ammo :

1. Click on the `New Scan` button
2. Scan for the value 20
3. Shoot a couple of times
4. Re-scan for the new value with `Next Scan`
5. Go back to 3 until there are only few results
6. Edit the value and watch the ammo grow

![Untitled](../images/MemoryEditing/Untitled%204.png)

Does anyone want to 1v1 me now? My aim is pretty bad but I am sure I can handle that 😎.

I am sure you don’t see any downside to my strategy, but the reality is that it is faulty. I can still take damage, which means that if there is anything in the game that can one-shot me, I will be killed anyways. Remember that we only edited the value, but the behavior of our variables keeps being the same as everyone else’s.

### Freezing

Now we are going to become true gods, and we are going to achieve that by modifying the behavior of the game.

It may seem evident to you that when you take damage in a game, it happens because some software engineer programmed that behavior, and you are absolutely right. Remember how I said that by knowing which 0s and 1s we modify, we can essentially do anything? That’s what we are going to do next, our goal is to find the code that decreases our health when we take damage, or our ammo when we shoot, and substitute to do nothing at all, so that we can’t take any damage or lose any ammo.

In order to accomplish this, we can right-click on one of the addresses and a menu will show up, we need to select there the option `Find out what writes to this address`. This will scan the binary (the game executable) and it will search for the code that modifies the value pointed by the address we are interested in.

![Untitled](../images/MemoryEditing/Untitled%205.png)

When we click on this option, we will see the instructions that modify the value in the address, but we first need to shoot in order to make the value change :

![Untitled](../images/MemoryEditing/Untitled%206.png)

As the image shows, there is an instruction in address `0x004C73EF` that decrements whatever is in the `eax` register. 

By double clicking the line 1, we get to see some more info about the assembly code, such as the registers’ state at that point, the instructions that come before and after the one we are looking for, and a couple extra options to see the stack’s view and the floating point registers (`S` and `F`).

![Untitled](../images/MemoryEditing/Untitled%207.png)

By double clicking once again on the decrement instruction, we get to see the whole assembly code, and we can modify it how we want.

A quick look into [Intel® 64 and IA-32 Architectures Software Developer’s Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html) will reveal that the `dec` function decrements its operand by 1 :

![Untitled](../images/MemoryEditing/Untitled%208.png)

As I mentioned, we will substitute the `dec` operation with a `nop` operation, which tells the processor to do absolutely nothing. This technique is known as freezing, since the value will now be unable to change (technically it can still increase but you know what I meant).

![Untitled](../images/MemoryEditing/Untitled%209.png)

![Untitled](../images/MemoryEditing/Untitled%2010.png)

We need 2 `NOP` because the decrement instruction occupies 2 bytes and we need to add the padding.

If you have been following the tutorial along with a VM opened and you have used the techniques that I have explained, you should now be enjoying infinite ammunition.

## Conclusion

When you have the binary, you have the power. Throughout this post we have seen that the memory of an executable can be edited as we please, and that if we know what we are doing, we can achieve pretty much anything. I have only showed you a couple of basic tricks, such as gaining infinite health or ammo, but I bet you imagine that you can accomplish anything that goes through your mind. I highly encourage you to open up Cheat Engine and give it a try! It is really fun and I am sure you will develop some new hacking skills.

Game hacking is a really fun way to learn and it can provide knowledge that is useful in many areas, you can learn stuff such as the internals of operating systems, how memory works, or even dive into the world of anti-cheats and how attackers use many sophisticated techniques to bypass them.

I am proud to announce you have reached the end of today’s post, I want you to know that feel really thankful and that if you want to read some more fun hacking articles you can search around the site, I am sure there will be something of your interest!

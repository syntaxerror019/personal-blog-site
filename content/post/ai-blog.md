---
title: "Can AI write it's own Blog Posts?"
date: 2024-07-31T18:12:40-04:00
draft: false
tags: ["AI", "News", "Server", "Media", "Publication"]
excerpt: "It can create art, it can write code, but can it handle the media?"
featured: true
---

## Introduction
Albeit, this article falls outwards of my typical writing style, which normally includes abundant code blocks and programming know-how, but I thought I would give it a try nevertheless. (omg this sounds AI written already i promise its not)

The idea of AI being able to write it's own social media posts, more specifically instructional posts such as tutorials and guides has plagued my brain from time to time for the past year. I mean it can write your English essays and it has the ability to generate life-like, photorealistic images just based on some textual inputs. How hard could writing a simple article be?  

I tried a similar thing a while back, using OpenAI's ChatGPT API and posting it's responses onto instructables.com using their (un)official API. This unfortunately did not work reliably and generated some strange feedback from readers, many saying my choice of wording was "odd."
The project was abandoned (like many of my others) and I had no intentions of picking it up again until...

## The Era of Hugo
After recently discovering Hugo and it's amazing open-source capabilities, I had another go at giving AI a media voice. With Hugo's easy to use command line and ChatGPT's ability to seamlessly respond in MarkDown, it seemed like a match made in heaven.  

All I had to do was write a little script that calls the LLM and then CTRL+C, CTRL+V it's responses into a new post. Then it was easy, I could simply (re)compile the Hugo site with the elementary `hugo` command and then use git to push new updates to the repo. Then, oh so beloved Vercel would handle the rest, making the site live to the public.

## The Issues
Now that AI has a voice, what will it do? Will it write about what I ask of it? Or will it generate nonsense that only confuses readers? Here was the first issue I came across; **fact-checking**  

How on Earth do I verify the validity of the bot's tutorials? The thought of forcing my friends to follow the possibly-rubbish guides to see their outcomes crossed my mind more than twice... The wisest choice of all is to have myself fact check them all individually. When a new article is written and published by AI, it will have a banner claiming that "This AI-written article has not yet been verified." This puts the reader in choice of proceeding or not. Ideally, I can have the articles checked within 1-2 business days. Afterwards, the banner will be removed and replaced by a similar one claiming that the article has been fact-checked by a human.

Although the reputability of the AI is most definetely my biggest concern, another pretty important problem that quickly surfaced was hosting.
Now this may not seem of such difficulty to most, (yes I hear you yelling 'VPS' over there calm down) finding a **free** VPS provider that doesn't have spontaneous uptime, and has a support team that speaks English is a bit harder than you might initially think. Luckily, OCI (Oracle Cloud Infratructures) came to the rescue with their Always-Free tier that allows VMs to be run 24/7 at the cost of limited performance, which should be fine for me.

Now this is definetely not all of the issues I have faced, nor will it be the last I face on this project. At the time of writing this article, I haven't even finished the project yet, which leaves abundant time for more issues to suface. I'll be sure to keep this article up-to-date on my latest failures and struggles though!
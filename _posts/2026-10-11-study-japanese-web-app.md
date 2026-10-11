---
title: 日本語を勉強しましょう！ (Let’s Study Japanese!)
date: 2026-10-11 16:25 +0200
category: tools
tags: typescript coding ai vibe_coding japanese
---
As part of my ongoing efforts to keep my Japanese skills at a reasonable level, I have been working through an interesting book, “Meetings at Toyota are 30 minutes” (トヨタの会議は30分)[^1]. However, it is full of kanji and concepts that I am not familiar with, and so I have been working through it carefully using Claude, cross-checking the translations in Google, and finally, spending time reviewing a lot of new vocabulary.

I looked around for websites to help me review, and one that I found useful is [MichiKanji](https://www.michikanji.com). The author of that site has implemented an easy-to-use website that has all 1,907 JLPT kanji, organized by level. Among the features that I have found useful is that for each of the kanji, he lists the stroke order. Even more helpfully, he  has implemented a player that animates the stroke order – this is very important because you can’t just write kanji in whatever order you feel like! Here is an [example](https://www.michikanji.com/kanji/勉) for the kanji 勉, the first character of the word 勉強 (benkyō) which means “study”.

But I found it to be tedious to have to cut and paste each kanji from a sentence, and then, for words like 勉強, which consists of 2 kanji, I would have to look up each one in isolation. This prompted me to think about building my own web-based application that would suit my particular study workflow.

And since the golden rule of vibe coding is: “Find the workflows in your life that are taking the most time and get a tool like Claude to automate them for you”, I knew exactly what to do.

## TL;DR
If you want to see the website, here it is: [njacobs5074.github.io/japanese-study-app](https://njacobs5074.github.io/japanese-study-app/text). The underlying code ([GitHub repo](https://github.com/njacobs5074/japanese-study-app)) is all open source.

## Ta-da! “Let’s study Japanese!”

My workflow for picking apart a sentence is as follows:
1. Type in the original passage from the book I’m reading (see above).
2. For each kanji or combination thereof, look it up in a Japanese-English dictionary.
3. Add it to an ever-growing list of such words (it’s a simple Markdown file I manage with iA Writer).
4. Study and review those words, often by writing it 25–30 times to help me remember it.

I reviewed the [About](https://www.michikanji.com/about) page on MichiKanji and noticed that the author, helpfully, attributes his data sources. 

That got me thinking, and so I fired up Claude Design and gave it this prompt:

> I want to build a web app that I can use to study Japanese. There are 2 sites whose data I think could be used to help build this. The first is KanjiVG (https://kanjivg.tagaini.net/index.html) that provides vector graphics for kanji and kana. These could be used to create a web-based cards that show the kanji on one side and then the kana and an English-language definition.
> 
> The second site is Tatoeba ([https://tatoeba.org](https://tatoeba.org/en/sentences/show_all_in/jpn/none)). It has an API that allows the retrieval of sample sentences in many languages, but in particular Japanese. I am thinking that I could use the text that I am currently studying (トヨタの会議は30分) by taking the words from it that I am not familiar with and drilling them in other sentences.

About an hour later, I had come up with [plan](https://claude.ai/artifact/QQNin6kTFPGNegu5qaHobL#page-dcb81aee8d83) that I was able to hand off to Claude Code.

A couple of hours after that and I had a rudimentary version of the app running locally. I spent some more time this morning refining it and then asked Claude to get it up and running as a GitHub Pages app. The link for the site is in the **TL;DR** above as well as on the left-hand sidebar menu (we’ll see how long that lasts ;)

## Vibe Coding

This site is, without doubt, an example of vibe coding. I have barely looked at the code for it and have relied on Claude Code’s abilities. I did, however, guide it with pretty detailed requirements, albeit those were what I would call “product requirements” and not technical ones.

Would I use this approach for any system I needed to build? Absolutely not. As a counterexample, for the [ISAM database manager]({%post_url 2026-04-12-first-encounters-with-claude %}) I built using Claude Code, I reviewed every line of code that it wrote. I both wanted to understand what it was building and have a reasonable expectation that the code was not that far from what I would have written myself (given the time).

But, in this case, I have not felt the need to do that. There are few, if any, consequences of the site breaking other than inconvenience. And while I’d like to think that it might be useful for others, it is mostly just for myself. Indeed, my motivation for posting it on the internet was so that I could use it on my phone and not just locally on my desktop computer.

In any case, if you find it useful, please let me know.

[^1]: [トヨタの会議は３０分―ＧＡＦＡＭやＢＡＴＨにも負けない最速・骨太のビジネスコミュニケーション術](https://www.kinokuniya.co.jp/f/dsg-01-9784799109496)
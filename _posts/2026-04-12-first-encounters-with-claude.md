---
title: First Encounters With Claude
date: 2026-04-12 13:30 +0200
category: coding
tags: rust coding ai
---
Recently, I started using Anthropic’s Claude Code in earnest. I’ve been using it to build a side project, partially as the basis for experimentation and partially because it’s a project I’ve always wanted to do: build a database manager. I talked about this in a previous [post]({% post_url 2024-11-28-learning-rust-with-a-purpose %}) and, honestly, I did not think it would take me this long to get started. Life happens. 

## What Did I Build? (`TL;DR`)
For those of you who want to skip to the code, see the following:
- [highlandcows](https://github.com/njacobs5074/highlandcows) - A Cargo workspace of Rust libraries under the highlandcows umbrella, starting with a persistent ISAM key/value store.
- [highlandcows_graphdb](https://github.com/njacobs5074/highlandcows_graphdb) - Label-based graph database built on top of highlandcows-isam

## How I Used Claude For This Project
I used Claude in 2 modes:
1. As a virtual software engineer who could architect and write code for me based on the prompts and guidance I provided. **I used this approach for the `highlandcows` project.**
2. As a teacher/coach who could help me write code. **I used this approach for the `highlandcows_graphdb` project.**

## A basic B-tree ISAM data manager
As the tagline for the `highlandcows` project states, this is an umbrella for a collection of Rust libraries. As of this post, I have only completed one crate: `highlandcows-isam`.

Here, I leaned heavily on Claude Code and let it do most of the work. Nevertheless, I did the work in phases. Thus, the first commit of the code implemented a 2 file solution: an index file that encodes the B-tree index and a data file that is nothing more than the data records laid out one after another.

I also asked Claude to implement the key/value pairs using Rust generic types. This meant that I could create a database of arbitrarily complex data elements. Claude proposed that I used the `serde` crate for serialization. When I asked it about the fact that the crate is no longer supported, it pointed out that while this is true, it is widely used in the Rust ecosystem. In any case, I was not overly worried as I am not trying to build a commercial product.

Additional capabilities were implemented iteratively as separate git [commits](https://github.com/njacobs5074/highlandcows/commits/main/). Claude made this process very natural and, depending on the complexity of the change, I would either approve via the Github web site or simply let Claude do the merge using the `gh` CLI.

I particularly enjoyed Claude’s ability to write documentation for me. Whether it was the notes for a commit, see [e4d6e51](https://github.com/njacobs5074/highlandcows/commit/e4d6e5178191506157df17d03c38a987eba6232a) or writing the code-level [documentation](https://njacobs5074.github.io/highlandcows/highlandcows_isam/), Claude handled this task easily, taking much of the drudgery out of building a new project.

As a hobby project, I cannot over-emphasize this last point. Claude very much acted as a multiplier, letting me focus on the high-level requirements of the project and, when the need (or my curiosity) arose, it was easy to switch into a “Tell me why you wrote the code that way?” mode.

In that vein, Claude  also helped me with several administrative tasks for the project:
- Built a proper CI/CD pipeline on Github that runs the tests, builds binaries, and manages releases
- Helped me to set up an account on crates.io where I could publish the Rust binaries for subsequent consumption

## Simple Graph Database
The second project I worked on with Claude was to build a non-trivial program that exercised the `highlandcows-isam` crate. It implements a basic graph database with the following data model for each node in the graph:

1. A key field for the record (a string)
2. An  optional text description
3. A list of 0 or more labels

Two nodes have an edge in common if they have the same label. Since nodes can have multiple labels, this is, strictly speaking, a hypergraph.

In stark contrast to the `highlandcows` project, here I used the Claude chat interface. One of the early prompts I gave it was that I wanted it to provide snippets for me as I discussed my requirements and goals for the project. This enabled me to do most of the coding, asking for assistance as the need arose. And since I am new to Rust, I asked for assistance frequently.

Finally, note that as of this writing, the API is only exercised in the test code for the project. Building a data loader and then a REPL are piece of work on my backlog.

# Final Notes
My goals, summarized in the following table, were modest.

| Goal?| Wanted| Reason |
|:-- |:--:|:-- |
| Improve my Rust?  | 🟢 | I have struggled to build non-trivial projects in Rust |
| Vibe Coding? | 🔴 | Wanted Claude’s help, but with a “human in the loop” |
| Commercial-grade database? | 🔴 | Nope: I expect to be the sole consumer |

Nevertheless, I felt that I learned a few important lessons, even from this “toy” project.

1. By following a strict “human in the loop” approach, I felt that I came away with a reasonable understanding of the code.
2. My costs, in terms of tokens, were modest. 
3. Claude proved to be a pretty good coach and mentor for a subject in which I had only basic knowledge.

## The Future
My expectation, along with the experiences of my team at work, leads me to believe that in the next 18-24 months, we will start to look at tools like Claude in much the same way we do high-level language compilers. In other words, while compilers do provide a way for us to read and review the assembly language that they generate, I think the last time I did that was in the 1980s when integrating hand-written assembly code into a C program was necessary. Thus, the need for “human-readable” computer code will become less important.

The non-determinism that LLMs demonstrate is a hard issue to build around, though. Determinism is a cornerstone of building commercial software. I suspect that the companies and teams who establish “good-enough” heuristics to overcome LLM non-determinism will be the winners, both in terms of velocity and reliability.

In the meantime, I enjoying having a virtual “colleague” who indulges my need to do coding as a hobby.
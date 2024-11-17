---
title: "The Unix CLI: You're Still The One"
date: 2018-05-01
author: Nick Jacobs
category: tools
tags: cli unix linux
description: Why the Unix Command Line still my go-to tool
---

> You're still the one -- who can scratch my itch<br>
> Still the one -- and I wouldn't switch<br>
> We're still having fun, and you're still the one<br>
<p style='text-align: center'>"Still The One" by Orleans, 1976</p>

# What Is The Unix Command Line Interface?
For the uninitiated, the Unix command line (aka the "shell") interface is a way of running
programs (aka commands) on a computer using only the keyboard.  The command line requires
you to type in the names of programs to run them. Here is a snippet of a command line
interaction:

```shell
[tic-tac-toe]$ ls
LICENSE      bower.json       package.json
README.md    karma.conf.js    test/
app/         node_modules/    tic-tac-toe.iml
```

Let's understand briefly what we're looking at here:

1.  `[tic-tac-toe]$` -- That's the "prompt", i.e. the computer's way of letting you know that it's ready for you to type a command.  Note that I've _customized_ my prompt to include my current directory.
2.  `ls` -- That's the bit that I typed.  `ls` is the Unix/Linux command to _list_ the contents of a directory.
3.  The rest is the output of the `ls` command and then computer prints the prompt again to let you know that you can type another command.

One could justifiably look at the above and question why go to all of this trouble just to view the contents of a directory (or "folder" as it's often called).  After all, there are easier ways to do this and they're just a mouse click or two away.  Despite this, computer users, and especially coders and system/network administrators use the command line, often to almost complete exclusion of modern graphical user interfaces.

Why is this?  As is often the case, a satisfying answer is not as simple as one would expect and is the topic of this essay.

_NB: I will use the term "Unix" interchangeably with "Linux" for most of this essay.  While purists may bridle at this distinction, with good reason, I hope that this "sloppiness" does not offend too many._

# A Bit Of History
Unix™, meaning the proprietary operating system written by AT&T Bell Laboratories' researchers, had its roots in the late 1960s when operating systems research was a big topic, both in academia and commercially.  Much of the development was done and completed by the mid 1970s.  User interface technology of that era was much different than now with teletype terminals (old-style green screens with keyboards aka TTYs) and line printers for hard copy.  Networking was a primitive affair afforded by low-speed modems.  I suspect that the widespread use of modems for Unix peripherals was in no small part due to the fact that AT&T had a monopoly on modems until the early 1970s.

The command line interface was the primary way to interact with the operating system.  Even the original Unix text editor, `ed`, was a line-oriented program[^1].

However, the authors[^2] of the Unix command line program (aka the "shell" or the "Bourne shell") were the same people who designed and built the Unix operating system.  Their vision of computing was revolutionary and their design continues to influence how software is written to this day.  

In fact, the command line, driven by a low-tech user interface, exposed a wealth of sophistication provided by the operating system in terms of scripting, batch jobs (i.e. programs that can be run on a specific schedule), and extensibility.  If you didn't like something, you could extend or even replace it.

This philosophy was taken to the next level in 1991 by Linus Torvalds when he released, as open source, the Linux kernel.[^3]  He, along with many other technical users of the Unix operating system were frustrated by AT&T's unwillingness to release their operating system as open source.  Linus Torvalds went on to succeed where a number of others had failed and as a result software developers and users have benefitted enormously in the subsequent decades.

As an aside, the willingness and availability of AT&T and the architects to spend time teaching and disseminating their work in academic institutions such as UC Berkeley during the 1970s played a key role in Unix's subsequent popularity.

## My journey with the Unix Command Line
My own use of the Unix command line started when a computer science student at NYU in the early 1980s.  A good friend also studying CS, very kindly gave me access to her Unix account which happened to be running the [BSD variant](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution) of Unix.  She directed me to a program called ["learn"](https://www.unix.com/man-page/bsd/1/LEARN/) that provided an interactive means of learning basic Unix commands, how files worked, and even `vi`, a fullscreen text editor.

Although the `learn` command helped me to understand the basic commands in just a few short sessions, more complete mastery of the shell occurred when I got a summer job at a small, local software company.  The founder of the company taught Unix and C programming to local corporates and during the course of that summer, he taught me not only the mechanics of programming on Unix but the philosophy that I have alluded to in the previous paragraphs.

His teachings were very straightforward and built on the core philosophy of write tools that cooperate and extend.  It is this philosophy that I will illustrate and encourage in the next few paragraphs.

## Starting With The Shell
If you own an Apple computer or have already installed Linux on a computer, you probably know that there is a command line interface available.  There is also a command line interface on Microsoft-based operating systems, but it differs considerably from the Unix-derived ones and I will not go into detail discussing it here.[^4]

At its most basic, the shell is simply a way of typing a command and getting a result.  The example at the beginning of this essay showed a directory listing.  However, I'm not going to dive into a tutorial on how to use the shell.  There are many such resources on the net and I doubt that I can do better.

Instead, what I want to direct your attention to is the typical facilities that modern shell programs provide.  For example, most shell programs, e.g. `bash`, `zsh`, `tcsh`, etc all provide high level facilities for:

- File I/O
- Text/data manipulation
- Parallel processing
- Remote program execution
- Job control

These facilities are then provided in what is commonly referred to as a REPL (Read-Evaluate-Print-Loop).  This is a precise definition of a user interface in which you provide a command to the program, the shell in this case, it interprets or evaluates that command, prints out the results and then waits for you to type a new command.

By the way, one can also extend this facility by saving useful series of commands into a file often called a script.  The script can then be executed as any other command that was previously installed on the computer.

### File I/O and Text/Data Manipulation
What makes this facility even more powerful is that the commands that you type and thus are executed by the shell on your behalf are for the most part not aware that their output is going to your screen.  Instead they simply write their output to a standard system destination called STDOUT (pronounced "standard out").  The shell arranges for the command's STDOUT to be connected to the terminal.  But it also makes it very easy to redirect that output to a file when the user specifies `> some-file`.  The `>` character signifies output redirection.  Similarly, you can tell the shell that you'd like the command to read its input from a file (as opposed to the keyboard) by simply specifying `< some-other-file` on the command line.  That program has no idea whether its input is from a file or the keyboard.

Thus, the shell provides a simple but powerful way to manage files and their content.

### Parallel Processing
Perhaps somewhat amazingly, the Unix shell has supported parallel processing since its earliest days.  The Unix kernel provides a facility called 'pipes' that allow a series of programs to be connected together such that each command in the pipeline's output is connected to the succeeding command's input.  These programs can then run in parallel and the pipeline completes when all of the output from the first command has been processed by the subsequent commands.  The shell exposes this facility using the '|' character, often referred to as the "pipe character" for pretty obvious reasons.

Note that the degree of parallelism exposed here is called "coarse-grained" parallelism.  The reason for this is that the smallest computational unit available to the shell is an entire command.  This is contrast to fine-grained parallelism where the computational unit could be a very lightweight component such as thread responsible for simply multiplying 2 numbers together.  Thus the parallelism that the shell exposes is appropriate for tasks such as transforming a text file or searching a log file for errors but would be inefficient for matrix multiplications.

Still this parallelism has been used in some places quite successfully, for example, compilers for the C programming language have been written to use pipes instead of temporary files to improve performance (keep in mind that at the time, hard drives were considerably slower than they are today).  I've written simple log monitoring tools that relied upon the ability to continuously scan system logs for specific entries such as errors and then hand off those entries to yet another program for further classification.

Again, the point of all of this is that underlying design of Unix and then the shell's leveraging of those capabilities has allowed successive generations of software developers to extend the platform in novel ways.


However, the above capabilities (I/O, data manipulation, and so on) are facilities that are provided by most programming languages and their attendant run-time environments.  What makes the shell so different? I think its power (and success) can be attributed to the following:

- Ubiquity -- The command line interface is available on all Unix/Linux computers.
- Immediacy -- There's a tight feedback loop when you use it.  If you mistype a command, you're told immediately of the error.  Modern shells have extensive in-place editing capabilities that make it easy to fix these errors.
- Power -- While the shell presents a high-level interface, it is a programming language with loop constructs, variables, functions, and so forth, it also allows you to directly manipulate the computer you're using at very low levels.
- Immersiveness -- The shell provides little in the way of distraction and I believe that given the deep level of concentration that is required for coding, this makes it a very productive way to interact with a computer.  There are no attempts by unrelated programs to "engage" you in other activities. :)

The shell is, by and large, a tool written by and designed for use by a highly technical audience, even within the tech community itself.  While at some point in Unix's history, there may have been a more general audience of moderately technical users, the advent of GUI applications has made other user interfaces more accessible and thus the command line has become largely the province of coders.

### File I/O and Text Manipulation
One of the main abstractions that operating systems provide is for reading and writing data.  By providing standard, albeit low-level interfaces to peripherals such as disk drives, they make it possible for programmers to largely ignore the differences between different manufacturers' products.

File I/O is a core operating system function and is the native persistence mechanism provided to applications.  However, in Unix a standard way of reading and writing file content[^6] is provided via 2 abstractions:

- Standard In - Often written as STDIN, this is a special file that a program can use for input of any kind.
- Standard Out - Often written as STDOUT, this is a special file that, you guessed it, a program can use for output, again of whatever it wants.

The Unix shell took this underlying abstraction and provided a very elegant and powerful syntax for using it. For example, simply using the '>' character after a command, one can _redirect_ the output of that command to a file.  Similarly, the input of a program can set by using the '<' character.  The program has no idea that this is being done, by the way, as reading and writing the keyboard or screen looks just like a file as well.

However where the real power comes in is that you can connect one program's output to another program's input using a Unix construct called a "pipe".  Pipes are a facility provided by the operating system but are exposed at the command line using the '|' character.

The ease with which this facility can be used can be demonstrated here.

```
$ tail -f /var/log/system.log | grep -i error
```

Without going into the details, the file `system.log` is has all kinds of useful messages written to it by various components in macOS.  But suppose that I just want to keep track of the errors?  And suppose I want to list those as they happen?

The above command does that and here is how it works.
1. `tail -f /var/log/system.log` -- Display the output of the system log "forever", thus the `-f` option to the command
2. `|` -- Pipe the output of `tail` command to another program...
3. `grep -i error` -- Search for all the instances of the word 'error' (irrespective of case, thus the `-i`) and display them on `grep`'s standard output, which in this case is the terminal where I typed all of the above.

Here is some sample output:

```
Mar 10 16:26:21 Nick-iMac secd[457]:  securityd_xpc_dictionary_handler cloudd[468] copy_matching Error Domain=NSOSStatusErrorDomain Code=-50 "query missing class name" (paramErr: error in user parameter list) UserInfo={NSDescription=query missing class name}
Mar 10 16:26:21 Nick-iMac cloudd[468]:  SecOSStatusWith error:[-50] Error Domain=NSOSStatusErrorDomain Code=-50 "query missing class name" (paramErr: error in user parameter list) UserInfo={NSDescription=query missing class name}
...
```

Ignoring for a moment the details of the about output, let us marvel at how powerful this is.  Having typed less than 100 characters, I can easily find out system errors almost instantly and this is just the beginning.  By using nothing more than the keyboard, there are 1000s of commands available whose outputs and inputs can be filtered and manipulated either on the fly or by saving them into text files that I can run in the same way I run any other program on the system.

In other words, my reward for investing the time to learn an arguably terse and cryptic interface is a fantastic return in productivity.

But when you consider what a coder does all day long, typing terse and cryptic text, this is a difference in kind not quality.

[^1]: This was not to change until Bill Joy, while a student at University California Berkeley, in 1977 wrote one of the first programs for Unix that ran in full-screen mode, a text editor called `vi` (for "visual"). https://en.wikipedia.org/wiki/Vi

[^2]: https://en.wikipedia.org/wiki/Unix

[^3]: https://en.wikipedia.org/wiki/Linux - You may have noticed that Unix is, in fact, a trademark.  Linus Torvalds, like many developers, was frustrated by the proprietary and closed nature of the Unix operating system, particularly the kernel.  Unlike many others, he wrote his own. :)

[^4]: If you want a reasonable facsimile of the Unix command line on Windows, I strongly recommend that you install [Cygwin](https://www.cygwin.com), an open source and very complete port of the Unix command line to Windows.

[^5]: For more information on Git, see https://en.wikipedia.org/wiki/Git.  I think that is telling that this is another Linus Torvalds-initiated technology.

[^6]: Historically, there has been a very text-centric aspect to application data on Unix, but there are examples where this approach has been used even for binary data. TODO: Find a pointer to the Unix C compiler's pipes architecture for its different stages. 

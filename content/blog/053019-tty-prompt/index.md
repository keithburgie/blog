---
title: Build a Better Ruby CLI with TTY::Prompt
date: "2019-05-30T01:57:30+00:00"
description: This is a custom description for SEO and Open Graph purposes, rather than the default generated excerpt. Simply add a description field to the frontmatter.
---

Using Terminal to get or send information can be a challenging experience for novice developers. Most have only interacted with their computer through a GUI and will quickly feel defeated by the empty box with a blinking cursor. After all, the only thing worse than nothing, is anything.

![Terminal After Help](./terminal-help.jpg)

Guiding a user through your program with selectable options or commands can provide a much better experience than asking them to type their own. Likewise, you may be able to build a better program if you're able to plan for the exact inputs you'll receive from your users.

##Introducing TTY::Prompt

[TTY-Prompt](https://github.com/piotrmurach/tty-prompt#ttyprompt- "TTY::Prompt Github") is described by its developer as "A beautiful and powerful interactive command line prompt." It is part of [an entire suite](https://piotrmurach.github.io/tty/ "TTY suite") of Ruby gems designed to help you build better, more functional command line apps. It provides several alternatives to ```gets.chomp``` for receiving input from your users, which would otherwise require many more lines of code to accomplish.

##How Do I Use It?
[The gem's GitHub page](https://github.com/piotrmurach/tty-prompt#ttyprompt- "TTY::Prompt documentation") has great documentation, but let's go through basic setup and some examples.

###1. In Terminal, `cd` into a project folder.
~~~ruby{numberLines: true}
# install the gem
gem install tty-prompt 
# create a new file
touch ttyprompt.md
~~~
###2. Open **ttyprompt.md** in your code editor
~~~ruby{numberLines: true}
# require the gem
require 'tty-prompt' 
# create a new instance
prompt = TTY::Prompt.new
~~~
###3. Do fun stuff!
~~~ruby{numberLines: true}
prompt.ask('What is your name?')
# => Get a string
~~~
~~~ruby{numberLines: true}
prompt.yes?('Do you like Ruby?')
# => (Y/n)
~~~
~~~ruby{numberLines: true}
prompt.mask("What is your password?")
# => ••••••••
~~~
###4. Finally, the reason we're here...
~~~ruby{numberLines: true}
question = "What's your favorite thing?"
choices = ["Apples", "Drugs", "Yarn"]
prompt.select(question, choices])
# => What's your favorite thing? (Use ↑/↓ arrow keys)
# => ‣ Apples
# =>   Drugs
# =>   Yarn
~~~
That last one, **prompt.select**, is really cool, because your user can use the up/down arrow keys and press enter to make their selection. You can pass that selection as a variable to do whatever you need to next. You can customize the cursor, change colors, build multi-selects, and lots more.

So, for the third and final time, if you're building a command-line application, [go check out the gem](https://github.com/piotrmurach/tty-prompt#ttyprompt- "TTY::Prompt docs"). I'm going to bed now because I spent too long setting up creating this blog instead of writing a blog post. No regrets!

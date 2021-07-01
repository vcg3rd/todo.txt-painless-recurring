README
=======

# Recurring Tasks for Todo.txt for Normal People

**tl:dl**.
Who needs an add-on script? Just uses aliases!

## Background
I am not a coder, developer, or programmer, but I am a hacker (read: tinkerer) who loves Linux, plaintext, and the command line.  I don't think I'm alone, but based on the documentation of open source projects one must wonder. Open source history in a nutshell is: share your code in case others find it useful; share your improvements.  The key here is Stallman's idea that "knowledge should be shared with other people who can benefit from it, and that important resources should be utilized rather than wasted."  Hackers didn't invent this.  Toolmakers did.  This isn't a new idea.  Make something that can benefit your family or clan or tribe and share it.  If they improve it, they share those improvements. The only thing the so-called Hacker Ethos did was apply sharing knowledge and applying standardization globally to software.

The key concept here is **useful** to **others** not just of your **tribe**.  If someone wants to write a program to use for himself, great!  More power to him.  If Larry Wall had written Perl to use on his computer and never shared it, fine.  But if you want **users** and you believe the code should be open and libre and modifiable, then it's no good if the only people who can use it are the people who can read your documentation.  And just like any specialized jargon (legal, medical, accounting) the only people who will understand it is your tribe.

And if hackers only want people to use their code who can read, understand, and modify it, fine.  Propitiatory, commercial software vendors actually want *as many users as possible*, and the less people who understand anything but the interface, and the less choice they have about customizing it or modifying **any** aspect of it the better.  Lock the software down. Lock the user in. Say what your want about Microsoft or Apple, and I'll agree, but one thing is for sure, though: They **want** people to use their software.  And software is **not** the only knowledge. In fact, terabytes of useful "knowledge is shared" and lots of valuable work done with propitiatory tools like MS Office and Adobe.

I have been using Linux since 1999 because it is secure, stable, modifiable, configurable, libre, and open, and I highly value those things.  I do nearly everything possible in plain text because it's future proof, convertible, readable in dozens of applications on every platform, easy to version control, isn't controlled in the cloud, portable, and more. Open source and plaintext are actually the most sane and best options for most knowledge sharing, and the most equitable for people who can't afford high end computers and software licenses and subscriptions. However, except on servers (mostly administered by those in the tribe), F/OSS will never make the contribution it could because coders would rather write for each other, both the code and the documentation.

## Enter Todo.txt (one example of many)

Because I like plaintext, I use Todo.txt.  Because I like the command line I use Todo.txt.  Because I can't so much as write a bash script, I can't do more than the most basic stuff, which is actually 90% sufficient.  I don't need complicated todo management like some people.  Hardly any task I have is complex or needs to be broken down or scheduled.  I use Emacs + Org-mode for a lot of writing, but I hardly use the agenda at all.  Todo.txt plus Remind meet 90% of my needs. Tasks in the former; events, appointments, birthdays, etc. in the latter.

My todo.txt file is really basic.  I picked todo.txt because it is plaintext, is easily used from the command line, isn't as complicated as taskwarrior, and has mobile apps.  The only time I need mobile is to add something when I am out or to shop.  Some items I keep on it like "milk" @shopping.  The one feature I want but can not get to work is recurring tasks, something a proprietary, subscription app implements effortlessly. Like in this example: 

![todo](https://user-images.githubusercontent.com/3229592/124200325-0f879400-daa3-11eb-81ee-39a918eea969.png)

Oh, sure, there's [repeat](https://github.com/drobertadams/todo.txt-cli-addons/tree/master/repeat), [recur](https://github.com/paulroub/todo.txt-recurring-tasks), and [ice_recur](https://github.com/rlpowell/todo-text-stuff/blob/master/ice_recur), but:

* ice_recur is a ruby file with three requires 'ice_cube','optimist' and 'fileutils', and I have no need of Ruby and no knowledge of it.

* recur is a perl script, assuming it's the right one.  The link for it in the [todo.txt wiki](https://github.com/todotxt/todo.txt-cli/wiki/Todo.sh-Add-on-Directory#recur-intelligently-add-recurring-tasks-todosh-ls1-some-task2-some-other-task3-a-third-task--2013-06-11-3-of-3-tasks-shown) is called todo.txt-recurring-tasks, and the wiki project page tells you to use the command 'bump' (see below), but the word bump does not appear in the [code](https://github.com/paulroub/todo.txt-recurring-tasks/blob/master/recur), and after installing it and trying to tell todo.txt to bump a task it returns the todo.txt usage message.

![recur](https://user-images.githubusercontent.com/3229592/124200425-51b0d580-daa3-11eb-826b-bc6dc3bc21ca.png)


* repeat has a Readme file that only says "An addon command for todo.txt-cli that marks an item done and immediately re-enters it," and it is only useful for scheduled tasks which I don't use.

* recur and ice_recur require a second file to sit in the same directory, and all three are 5-10 years old.  Maybe that's OK for a Ruby and Perl script but todo.txt itself's latest version is 08/2020.

## My Solution
After coming back every year or so and trying to get any of them to work, and failing, I finally asked myself how would I implement this with what I have?  We'll the only add-on I use is the original author's, Gina, [add and do](https://github.com/todotxt/todo.txt-cli/blob/addons/.todo.actions.d/addx).  With this add-on sitting in my .todo.actions.d directory, the answer was simple. I already have this in my .bashrc:

```
export TODOTXT_DEFAULT_ACTION=ls
alias t="~/todo.sh -d ~/todo.cfg"

```
So, I added two more aliases:

```
alias tx="t add x"
alias ta="t archive"

```

## How it Works
1. You put recurring tasks in your todo.txt file.
2. You tag them how you want them to recur @daily, @mon, @weekly, @monthly
3. You do ls @daily
4. You see Oh...I need to scoop the litter.  Have I?
5. If yes you type tx "scoop litter".  Now a new entry is created with a x in front of it and the original entry is left alone.
6. Now, type ta and that, and any other done tasks, get archived.  Scoop litter is now in done.txt and yet it's still in todo.txt.
7. Finally I have a cron job for a script that is run at 11:59 on the last day of the month that renames done.txt to done_%Y%m.txt and then touches done.txt.

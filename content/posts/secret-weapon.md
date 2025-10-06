+++
date = '2025-10-05T16:50:48-07:00'
draft = false
title = 'My Secret Weapon'
+++
## tl;dr
I use the following to create and maintain a daily journal of plain markdown files:

* A terminal with hotkey window for fast global access to a terminal session ([iTerm2](https://iterm2.com/))
* A terminal editor running in that window ([NeoVim](https://neovim.io/), tmux recommended)
* Editor plugin for daily journal features ([VimWiki](https://github.com/vimwiki/vimwiki))
* Extra shortcuts for checklists and timestamps (custom vim mappings)
* Editor plugin for fuzzy search ([fzf.vim](https://github.com/junegunn/fzf.vim))

With that setup, I pop open my hotkey terminal to capture notes about what I'm doing through the day _constantly_.  The key is that accessing and dismissing the journal is *very low friction*.


## Background
I've worked as a software engineer in large companies for over 20 years.  During that time I had a lot of moments where I reached the end of a work day or work week and honestly did not know where all my time went.  Software engineering work can be like that: you spend hours or days researching some technology, studying some system that you have to work on, reviewing others' code, reading or writing design documents, interviewing people and writing feedback, meeting with people to get on the same page about things, responding to random questions or support calls, and sometimes even writing code.  After all that, it can be hard to remember everything that happened during that time... or sometimes in my case, remember anything (my memory for events is often not great).

At some point (2017-ish) I got frustrated with not remembering where my time went, so I made a system for capturing work events in a journal.  It's evolved over the years, but I've found my journal system useful enough to keep using it since then.  A few coworkers have asked me about it over the years, and when I'd show them I often said "I should really write this down somewhere".  So here we are.


## Step 1: Set up terminal application with hotkey window

I use a Mac and use [iTerm2](https://iterm2.com/) as my terminal application.  (If you use [Homebrew](https://brew.sh/), `brew install iterm2`)  The Hotkey Window feature is one of the reasons I use it instead of Apple's built-in Terminal.app. https://iterm2.com/features.html describes the Hotkey Window feature.  The short explanation is: From basically anywhere in my system, when I press my mapping keystroke (I picked `<ctrl><option><command><space>`) a terminal window zips down from the top of my screen and takes keyboard focus.  Clicking or switching away from the window causes the window to disappear again.  When I first learned about the Hotkey Window feature I felt it was a gimmick, but today it's a major reason why I use iTerm2.

Set up a Hotkey Window profile with whatever global shortcut you like.  I use `<ctrl><option><command><space>` so I can just mash all of those buttons at the bottom of the kwyboard to open the window.  (An aside: I'm a fan of using the space bar for global and frequently-used shortcuts since it's the largest key on the keyboard and very convenient to press.  I set `<space>` as my "leader" key in Vim editor for custom shortcuts, `<ctrl><option><space>` is mapped to my application launcher - I use [Quicksilver](https://qsapp.com/) but [Alfred](https://www.alfredapp.com/) is also good - and `<option><space>` is mapped to ChatGPT or Claude app global shortcut for LLM.)

My hotkey window terminal almost always has my editor open to a specific text file: my daily journal file.

{{< mp4video src="/video/journal-secret-weapon/terminal-hotkey-window-demo.mp4"
             caption="screen capture demo of iterm2 hotkey window" >}}

## Step 2: Set up terminal text editor with daily journal text file

I use [NeoVim](https://neovim.io/) as my primary editor for most cases, but any terminal text editor will do here.  Within NeoVim I use the [VimWiki](https://github.com/vimwiki/vimwiki) plugin, which has some handy shortcuts - including a daily diary feature.  (VimWiki also works fine with regular Vim.)

I configure VimWiki to use Markdown instead of its default wiki syntax, so I end up with one Markdown text file per day.  I have a handful of shortcuts defined, starting with one to open today's diary file or create it if it doesn't yet exist.

If you aren't a Vim user or just don't want to use that plugin, it wouldn't be tough to make a script or mapping to create a daily file without VimWiki.

## Step 3: Type things that happen

Usually the first thing I do when I log into work each morning is open vim (inside a tmux session that I name "notes") inside the iTerm2 Hotkey Window, and create my diary file for the day.

One of my shortcuts creates the template for my daily file, which has a "JOURNAL" section at the top and "TODO" section at the bottom:

```
## JOURNAL
*


## TODO
* [ ]
```

I generally fill in the TODO section at the bottom with the list of things I plan to do that day, including meetings scheduled in my calendar.  I use VimWiki's checklist features for that section: `<ctrl><space>` toggles a checklist item as completed or not.

Then at the top JOURNAL section, I write notes about what I do.  I try not to make these super-detailed - it's more like a nested bullet list.  It has things like what meetings I was in that day, what code I was writing or debugging, URLs of docs I read.  (I have a shortcut that copies an item from the TODO section into the JOURNAL section, which is not vital but something I do often enough that a shortcut helps.)

I use this space as a scratch pad for messages I write to people (or LLMs nowadays), and for notes and comments while I'm working through problems.

I don't spend time making this very structured.  It's mostly bullet lists of things I'm doing, and mostly in chronological order as it happens.  I check off items in the TODO checklist as I get them done, or carry them over to the next day if I don't.  If I carry a TODO over too often, I eventually ask myself why - or lose patience with procrastinating on that thing and get it done.


## Step 4: Timestamp things

For years I used my daily journal at this level and it worked fine.  What took it to the next level was creating a [shortcut](https://github.com/kjhaber/dotfiles/blob/6358e0f4dd4e2cef0920a0924560d7aba62b30c7/.config/nvim/lua/plugins/vimwiki.lua#L74) to write the current timestamp at the end of the current line, or updating the timestamp if it wasn't present.

Example:
```
* start writing Timestamp things section [20:41]
```
I type `<space>wm` (mnemonic: w=Wiki, m=Minute) to stick the current hour and minute to the end of the line.  Still low friction.  And adding the timestamp is automatic when using the shortcut to copy events from TODO to JOURNAL.  (It might have been neater and tidier to prepend the timestamp to the start of the line, and someday I might change it to that.  That said, I've found that any neatness and tidiness is lost quickly when timestamping nested bullet points anyway.  So for now it's at the end to emphasize the activity over the timestamp.)

Example:
```
* 2pm Staff Meeting [14:01]
```

Once I started using timestamps, it became easier to capture:

  - when I log in for the day (automatic when I do the InitDiary shortcut)
  - when I start a task
  - when I end a task (usually with a "done" line)
  - when I did various intermediate steps in a longer activity
  - when I log out for the day


And so on.  I found this did a few things:

1) I was better able to recall what I did each day, and I had a better record of when things happened and how long they really took.  That gave me an opportunity to realize where things were taking too long, and come up with ways to improve.

2) It's helped my self-discipline.  If I go too long in a day without writing a line with a timestamp, I start to feel like I'm slacking off or losing focus.  I encourage adding lines for when you take breaks and timestamp those!  And then another timestamp for when you resume.  For me it makes breaks more deliberate, which is a good thing.  When working from home since the COVID-19 pandemic, I have an easier time noticing when my lunch or breaks have gone short or long.

3) It's helped my work-life balance.  In a sense it's a little like "clocking in" or out, though even with this system I'm not super strict about starting and stopping work at an exact time.  But having a record of times where I did put in a couple of long days in a week helped me feel better about logging out a little earlier on other days, rather than just going from my feelings and bad memory.  For me that has been very helpful.

At some point I may make tools or start using LLMs for deeper analysis of my time spent using the timestamps.  For now it's been valuable just capturing them and using them immediately as described though.

For what it's worth, I keep a vimwiki journal like this on my personal machine, and don't use timestamps in that.  I don't need or want productivity hacks for my home life.


{{< mp4video src="/video/journal-secret-weapon/journal-init-demo.mp4"
             caption="Example of initializing journal: open iTerm hotkey window and create a tmux session, open vim, run `:InitDiary` to start a journal, and use timestamp and checklist shortcuts on journal notes." >}}


## Step 5: Search for things as needed
Just the act of writing notes helps me remember things, but it's even more powerful to be able to search.  I mentioned making the notes searchable.  For this I use [fzf.vim](https://github.com/junegunn/fzf.vim) though there are other fuzzy search solutions out there.  I generally use two fzf shortcuts: one for searching filenames, and one for searching content.  The fzf.vim plugin lets you pop open a floating window and do a live fuzzy search.

Fuzzy-searching filenames is handy to jump straight to a date.  This is super easy since the daily diary files are just named for the day (`2025-09-14.md`).  I have this mapped to `<ctrl>p`, mostly for historical reasons.  A long time ago there was a plugin for fuzzy-searching files named [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) that was popular in vim circles.  I used it and kept that mapping when switching to other fuzzy-searching solutions.)

(Aside: another way I like to jump to specific dates is with the [calendar.vim](https://github.com/mattn/calendar-vim) plugin, which has VimWiki diary integration built-in.  Invoking the plugin opens a buffer with a calendar window; click on a date and it opens the diary file for that day!)

Searching content is even more powerful.  (I have this mapped to `<leader>//`.  I'll talk about my vim mappings in a different post, but short version is forward slash is the usual vim key for searching, and most of my leader mappings have two or more characters to make it easier to organize and remember a lot of shortcuts.)

As I write things in my daily journal, I do a few things to make my freeform notes more searchable:
a) Link to URLs often.
b) When encountering a new acronym (which happens a lot in big companies), add a line with that acronym and its meaning.

Example:
```
  * TLA = Three Letter Acronym
```
Then when I search for acronyms later, I fuzzy search for the acronym plus an "=" and voila.

c) Refer to people by their internal company ids.  The first time you meet someone, add a line with that person's internal id, full name, and title/team/etc.

Example:
```
* 9am Project meeting [09:01]
  - attendees: khaber, shamwich
    - shamwich: Sam Hamwich (Software Engineer - Fictional Coworker)
```

Doing this consistently makes it easy to find past interactions with people, and in some cases faster to look up someone's name than using the company directory or Slack profile.  Also use those ids when writing the list of attendees of meetings (which is a good habit for meeting notes in general).

c) Invent tags for various events, especially for events you want to be able to count later.  For example, I currently work remotely, but on days when I go into the office I add a line consisting of "@office" to the start of my daily journal.  Then I can use normal unix tools to count the number of days I've been in the office: `grep '@office' ./* | wc -l`  (I suspect it will eventually be useful for doing deeper analysis on journal entries from days worked in office vs. days worked at home.  How many hours did I really work, vs losing to commute?  What valuable interactions happened in office that wouldn't have happened working from home?)

But mostly when I fuzzy search, I have some idea of the timeframe and words to look for.  It's clearly not a total substitute for actual document searches, but even as a mostly unstructured pile of notes it turns into a useful knowledge base fairly quickly.  And it is so handy being able to pop up a search for that meeting you had with a certain person a few months ago, or that one document or tool you ran into last week, or that cryptic Kubernetes command you found to fix an auth annoyance, etc. etc.  Your fuzzy searching will often yield results much faster than asking ChatGPT or running an internal search query.



## Conclusion

In my [vim config](https://github.com/kjhaber/dotfiles/blob/main/.config/nvim/lua/plugins/vimwiki.lua) I have a number of other settings and shortcuts for this system not discussed here.  This post gives the main idea though.  I encourage trying some of the ideas and see what works best for you.

I really do think of this journal system as a secret weapon at my day job.  It has helped me gain awareness of wasted time and helped me improved my memory and self-discipline.  It has also quickly grown into a useful personal knowledge base for workplace knowledge.  Occasionally it has saved me from trouble and made me look smart and well-organized in front of my boss and coworkers too.  And sometimes just writing down my thoughts in the journal helps me to focus my efforts more effectively too.

A word of warning: don't treat time spent writing in this journal as being directly productive.  Writing in the journal takes time that could be spent doing something else.  If you are prone to procrastination (as I sometimes am), it can be easy to fall into the trap of writing complete notes for every meeting and project and organizing notes instead of making actual forward progress on your tasks and goals.  Be mindful of keeping the note-taking quick: the journal is a means to an end, not a goal in itself.

I hope this proves useful to someone.  Good luck!


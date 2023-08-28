---
title: "Vim for everyone!"
date: 2022-01-09T22:00:04-05:00
draft: false
author: "Said Neder"
tags: ["Vim"]
toc: true
categories: "Productivity"
image: "images/neovim.png"
---

## Intro

This is a vim-neovim tutorial, is a highly-configurable text editor that you can make it as you wish, and the way to use it is only with the keyboard and keybindings and learning it will improve your productivity at a whole other level, and it is included in every UNIX system and Mac OS, so if you configure servers remotely or work a lot in the terminal is really important to use vim, and you can add plugins and make a lot of cool stuff with it!

## 1.0 Installing vim
The first step into the rabbit hole, here it goes!

**Debian/Ubuntu**
`sudo apt in vim`

**Fedora/RHEL**
`sudo dnf in vim`

**Arch Linux**
`sudo pacman -S vim`

## 1.1 Vim modes
Vim has three modes and each one of the three you can escape them with ESC:

- Normal mode: You use it to navegate in the editor, you should be in this mode the most of the time.
- Insert mode: In this mode you can type text into the file, type `I` to enter insert mode.
- Visual mode: In this mode you can select large chunks of text, to copy, paste, cut, etc...

## 1.2 Quitting vim
There are different ways of quitting vim, but these are the most important:

- `:q!` = Quit and don't save changes
- `:q` = Quit
- `:wq`= Write and quit
- `:x` = Exits the file, if the file has changes it will save them.

## 1.3 Useful shortcuts to use
- To quickly go to the initial character of the line = `0`
- To go to the first line of the document = `gg`
- To go to the last line of the document = `G` (shift+g)
- To go to a particular line of the document = `10G` (10 is the number of the line you want to go)
- To go to the most near parenthesis use = `%`
- To execute a command inside vim = `:!neofetch`
- Vim can help you if you don't remember a command! just use `ctrl+d` when writing a command, for example you want to write and quit but don't remember how, just press the inital letter and then `ctrl+d` the `:w`

## 1.4 Deleting, undoing and redoing!
- To delete the character that the cursor is selecting = `x`
if you want to delete more than one character you can by doing so = `4x` (Knowing that the 4 is the quantity of characters you want to be deleted)
- To undo what you did use = `u`
- The same principal tackles undo with multiple times = `3u`
- To redo, just use = `ctrl+r`
- To delete word by word use = `dw`
- To delete line by line use = `dd`
- To delete backawards word use = `db`
- To delete from the cursor until the end of the line use = `D`

## 1.5 Navigating more in-depth
- To navigate word by word use = `w` (ends of the beginning of each word)
- If we want to navigate word by word ending on the last character of the word use = `e`
- Mix it up! navigate between quantity of words = `3w`
- Go backawards = `b`
- Navigate by visual lines! Vim is made to navigate (h,j,k,l) in logical lines, where it's supposed to help when inserting, but if you want to navigate for reading use = `gj` (gj being visual down)
- Using `w`, `b` and `e` more in-depth: when navigating with this keys, the commas, parenthesis, etc, are counted as a word, but if we don't want that just capitalize it! = `W` `B` `E`
- Forwarding to navigate to a specific character, for example move the cursor where a letter t is = `f` example = `ft`
- Forwarding in reverse! = `F`
- Forwarding until, forward until you find a certain character, landing the cursor before that character = `t`
- Forwarding until in reverse = `T`
- Inserting without caring about blank characters, inserting to the first non-blank character = `shift+i`
- Navigating between blocks of text, being next blank line use = `}`
- Navigating between blocks of text, being the previous blank line use = `{`

## 1.6 Replacing, appending and changing
- You mistyped something? Not to worry with the replace function! it replaces the character where the cursor is and then press it replacement = `r`
- If you want to change something, you can with the change function, where it changes the character before the cursor, for example let's change a word, remember to exit out of insert mode, let's use = `ce`
- Let's change a entire line! use = `C`
- Let's append to a line, since inserting is always before the cursor, append to insert after the cursor = `a`
- Append to the last character of the line = `shift+a`
- To delete a character and start in insert mode use = `s`
- To delete a entire line and start in insert mode use = `S`
- To insert on the line below use = `o`
- To insert on the line above being on the line that you are use = `O`

## 1.7 Yanking and putting!
- Yanking is copying in vim, so for example let's yank a word! = `yw` 
- What if you want to delete something and put in on other line? = `p` (For example, you delete a line `dd` and put it three lines below `p`)
- Yank from the cursor till the end of the line with = `Y`
- Remember that cutting in vim is just deleting and putting it

## 1.8 Searching in your document
- Press `/` and then the word you want to search!
- If you have multiple instances of that word, you can use `n` to go next or 
`N` to go backawards
- When searching to go back to where you started, and if pressed again to the beginning of the file, use = `ctrl+o` 
- If I want to go forward where I was use = `ctrl+i`
- Replace your finding! press `:` and then `s/finding/replacement/g` following the formula!
- Replacing in the entire document, formula = `%s/finding/replacement/g`
- Replacing in the entire document with a yes/no prompt = `%s/oldstring/newstring/gc`
- Replacing each instance between lines = `3,9s/oldstring/newstring/g`


## Reminder: Remember to mix it up!
You can mix numbers with hotkeys, for example if you want to delete 10 lines you aren't going to press `dd` 10 times, just multiply it! `10dd` and so on and so forth with the others! Mix it up!

## Outro
This is for now! these are the essential vim hotkeys and commands to survive and to start into the rabbit hole, expect more of vim in my blog thank you for reading!

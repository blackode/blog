---
layout: post
title: Top 4 Vim Commands for Novice
---

vim is loves the guy who loves it

Vim loves the guy who loves it and hates the guy who hates it. Vim lovers do not love a girl , they love code.

Many of the **Vimmers** might be knowing these commands, sorry if I upset you but I am writing this for **vimmers** who just sow their seeds in the field of Vim and waiting for the treasures ……..

### 1 . command {:w}

Even though this is the most useful command and typed command in the vim editor , there are the cases where you open the root file or protected file with out **sudo** so you can’t write the file then,so you have close the file and re open it as **sudo** and do your space work over the file, but actually there is the command for the whole action to be fulfilled.

> :w !sudo tee %

[https://github.com/blackode/vim_gifs/raw/master/assets/small.gif](https://github.com/blackode/vim_gifs/raw/master/assets/small.gif)

### 2 . command {:wa}

This command is nothing more special than :**w **command ,semi unlike the :w command it writes all the open buffer files which you have edited and for got to save and switched to another file buffer. I usually experience this while working with multiple files in a project . This also useful when you have a doubt of whether you have saved a file or ont . So, this command is more useful in certain situations. I think every programmer might have faced this situation.

### 3. command {:q!}

This command is used to force quit the file.Some times you edit the file and trying to quit the file with out writing to the file ,at that time vim don’t allow you do that, so you have to add the** ! **mark after **q **as **:q!** so the file gets closed left with previous state.

### 4. command {:r}

The :r command is used to read the another file data or system output into the buffer.

#### Examples

#### :r file1.txt

Above one reads the all the lines of data in file1.txt into the current buffer file.

#### :r !ls

Here the ls command output is will be written into the current buffer file.

We can use this command for multiple uses like inserting current d**ate and time **you can use the command like **:r !date. **If you this is command in a correct format, You can save a lot of time

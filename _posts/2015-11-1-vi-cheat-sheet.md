---
layout: blog_item
title:  "Vi Cheat Sheet"
date:   2015-11-1 09:39:10
author: Arr0way
description: 'Vim cheat sheet, a collection of Vi / Vim commands and tips in a nicely presented cheat sheet.'
categories: [cheat-sheet]
tags:
- 'Vim'
- 'vi'
- 'Linux'
---

* list element with functor item
{:toc}

A collection of Vi commands in a cheat sheet, handy reference document for learning / remembering Vi commands. I refer to Vim / Vi as the same thing in this document, but in most modern Linux distros vi is often a symlink to vim.

It's worth learning Vi as it's installed on pretty much every Linux base system out there.

If you're learning Vi, you might want to check out [Vim Adventures](http://vim-adventures.com) - an online adventure game that uses Vi commands.

## Vim Resources

* [Vi Cheat Sheet](https://highon.coffee/blog/vi-cheat-sheet)
* [Good looking Vim Themes](http://themebow.com)
* [NullMode's personal Vim Settings](https://github.com/NullMode/vim)
* [Vim Casts](http://vimcasts.org)

Enjoy the cheat sheet :)

## Vi Insert mode & Command Mode

Vi has two basic modes, **insert mode** - used for entering text and **command mode**, used for entering commands. See the tip section below for switching between each mode.

### Vi Insert Mode

Enter vi insert mode, insert mode is used for inserting text.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>i</code></p>
      </td>
      <td>
            <p>Enter insert mode from command mode.</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

<div class="note tip">
  <h5>Vi Command Mode - Vi Insert Mode</h5>
  <p>Vi has two modes, <b>insert mode</b> for inserting text and <b>command mode</b> a common mistake is attempting to edit in command mode. If you are unsure on what mode Vi is using double tap escape (enters command mode) and then hit "i" if you wish to enter insert mode.</p>
</div>



### Vi Command Mode

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vim Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>Esc</code></p>
      </td>
      <td>
            <p>Hit escape to enter command mode.</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi File Navigvation

Basic file navigation, how to move up, down, left and right.

<div class="note tip">
  <h5>Arrow Keys</h5>
  <p>Modern Vi / Vim editors will allow you to use the arrow keys, but it's worth learning the correct way to navigate vi without using the arrow keys in case you come across Vi command line or a shell that doesn't like arrow keys.</p>
</div>

### Move up, down, left and right in Vim

You'll need to be in command mode for these commands, navigation in vi uses **h** **j** **k** **l**

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vim Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>h</code></p>
      </td>
      <td>
            <p>Move left - easy to remember h key is on the left</p>
      </td>
    </tr>
    <tr>
    <td>
      <p><code>j</code></p>
    </td>
    <td>
          <p>Move down - I remember it with j(d)own for down</p>
    </td>
  </tr>

   <tr>
   <td>
     <p><code>k</code></p>
   </td>
   <td>
         <p>Move up - k for up - I remember it with (k)up</p>
   </td>
 </tr>

   <tr>
   <td>
   <p><code>l</code></p>
   </td>
   <td>
       <p>Move right - l is on the right side of hjkl and moves you right</p>
    </td>
   </tr>

    </tbody>
</table>
</div>

### Vi Page Down

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vim Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>Ctrl+F </code></p>
      </td>
      <td>
            <p>Vi move forward a page</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Half a Page Down

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>Ctrl+D </code></p>
      </td>
      <td>
            <p>Vi move half a page down</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Page Up

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>Ctrl+B </code></p>
      </td>
      <td>
            <p>Vi move up a page</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Half a Page Up

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>Ctrl+U </code></p>
      </td>
      <td>
            <p>Vi move up half a page</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## More Advanced Ways of Entering Insert Mode

### Vi Insert Text at Start of the Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>I</code></p>
      </td>
      <td>
            <p>Insert text at the beginning of the line</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Insert Text at the end of the Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>A</code></p>
      </td>
      <td>
            <p>Appends text at the end of the line</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Append text to the right of the Cursor

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>a</code></p>
      </td>
      <td>
            <p>Appends text to the right of the cursor</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Begin a new line below

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>o</code></p>
      </td>
      <td>
            <p>Begin a new line, below the current line</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi replace line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>O</code></p>
      </td>
      <td>
            <p>Removes line, and allows you to type a new line in it's place</p>
      </td>
    </tr>
    </tbody>
</table>
</div>


## Vi Replace

### Change a Word in Vi

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>cw</code></p>
      </td>
      <td>
            <p>Replaces a single word, place cursor on first letter and hit cw (Change Word)</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Replace line, but not wrapped text

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>c$</code></p>
      </td>
      <td>
            <p>Replaces the current line but doesn’t extend to change the rest of a wrapped sentence on the screen</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Replace Character

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>r</code></p>
      </td>
      <td>
            <p>Replaces only the character under the cursor</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Replace

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>R</code></p>
      </td>
      <td>
            <p>Replaces over the top of existing text, until the user hits return.</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi Delete

### Vi Delete Single Character After the Cursor

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>x</code></p>
      </td>
      <td>
            <p>Vi deletes single character after the cursor</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Delete Character before the Cursor

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>X</code></p>
      </td>
      <td>
            <p>Vi deletes character before the cursor.</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Delete Word

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>dw</code></p>
      </td>
      <td>
            <p>Vi Delete Word, deleted the word under the cursor, from the curosr position onward</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi Delete Line commands

### Vi Delete Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>dd</code></p>
      </td>
      <td>
            <p>Delete the current line in Vi</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Delete until end of Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>D</code></p>
      </td>
      <td>
            <p>Deletes from cursor to end of line</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Delete to end of screen

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>dL</code></p>
      </td>
      <td>
            <p>Deletes from cursor to end of screen</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Delete to end of file

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>dG</code></p>
      </td>
      <td>
            <p>Deletes from cursor to end of file</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Delete From Cursor To Start of Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>d^</code></p>
      </td>
      <td>
            <p>Deletes from cursor to start of line</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi Copy and Paste

### Vi Copy Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>yy</code></p>
      </td>
      <td>
            <p>Copies current line into unnamed buffer</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Copy 3 Lines of Text

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>3yy</code></p>
      </td>
      <td>
            <p>Copy 3 lines of text into unnamed buffer</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Copy Word

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>yw</code></p>
      </td>
      <td>
            <p>Copy word under cursor into unnamed buffer</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Copy 3 Words

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>3yw</code></p>
      </td>
      <td>
            <p>Copy 3 words into unnamed buffer</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Paste Commands

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>P</code></p>
      </td>
      <td>
            <p>Copy contents of unamed buffer to right of cursor</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>p</code></p>
      </td>
      <td>
            <p>Copy contents of unamed buffer to left of cursor</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi Search Commands

### Vi Search forward

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>N</code></p>
      </td>
      <td>
            <p>Vi search forward in file</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search Back

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>SHIFT+N</code></p>
      </td>
      <td>
            <p>Vi search backward in file</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi Search and Replace Commands

### Vi Search and Replace First Instance

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:s/find-string/replace-string/</code></p>
      </td>
      <td>
            <p>Vi search and replace first instance of specified string</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search and Replace on a Single Line

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:s/find-string/replace-string/g</code></p>
      </td>
      <td>
            <p>Vi search and replace all instances of specified string on current line</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search and Replace Entire File

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:%s/find-string/replace-string/g</code></p>
      </td>
      <td>
            <p>Vi search and replace all instances of specified string for entire file</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search for part of a Word

A fuzzy search allows you to find something that you only know part of, for example if you wanted to find all instances of lines starting with the word “Picard” you would use the following:

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>/^Picard</code></p>
      </td>
      <td>
            <p>Vi search within file words starting with Picard</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search for words ending with $string

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>/worf$</code></p>
      </td>
      <td>
            <p>Vi search within file for word engine with worf</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search for Metacharacters

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>/\*</code></p>
      </td>
      <td>
            <p>Vi search within file for metacharacters like, * </p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Exact Match Search Only

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>/star\.</code></p>
      </td>
      <td>
            <p>Vi exact search only, will return instances of "star only", not starfleet or star-trek</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search for a range of Strings

Helpful for finding version numbers in text files.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>/v2.[1-9]</code></p>
      </td>
      <td>
            <p>Vi search for a range, this example would return all v2.1-9 instances within the file, e.g. v2.4 v2.7 etc</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Search for Upper and Lowercase

Search for upper and lowercase strings in Vi.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>/ [tT] [hH [eE]</code></p>
      </td>
      <td>
            <p>Vi search upper or lowercase strings, this example would return any instance of the word 'the'. e.g. The, THE, tHE, tHe </p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Advanced Vi commands

### Vi View Options

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:set all</code></p>
      </td>
      <td>
            <p>Lists all Vi options</p>
      </td>
    </tr>
    </tbody>
</table>
</div>


### Vi Run Shell Commands

Run shell commands from  Vi.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:! ls -l</code></p>
      </td>
      <td>
            <p>Run shell command from Vi, in this example ls -l is executed</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Joining Lines

Backspace doesn't always work...

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>SHIFT+J</code></p>
      </td>
      <td>
            <p>Position the cursor in either line you wish to join, and press <code>SHIFT+J</code></p>
      </td>
    </tr>
    </tbody>
</table>
</div>


### Vi Split Windows

Useful for comparing files, to switch between windows press <code>SHIFT+W</code>

#### Vi Split Window Horizontally

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:split</code></p>
      </td>
      <td>
            <p>Split window Horizontally in Vi</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

#### Vi Split Window Virtically

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:vsplit</code></p>
      </td>
      <td>
            <p>Split window Virtically in Vi</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Close All Split Windows

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:only</code></p>
      </td>
      <td>
            <p>Closes all split windows and focuses on the primary window</p>
      </td>
    </tr>
    </tbody>
</table>
</div>


## Vi Save commands

### How to Save in Vi

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:w</code></p>
      </td>
      <td>
            <p>Writes the file to disk</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Save and Exit

How to save and exit Vi, personally I use <code>:wq</code>

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:wq</code></p>
      </td>
      <td>
            <p>Save and exit Vi</p>
      </td>
    </tr>

    </tr>

      <tr>
      <td>
        <p><code>:x</code></p>
      </td>
      <td>
            <p>Exit - Vi will prompt and ask if you wish to save</p>
      </td>
    </tr>

    </tr>
      <tr>
      <td>
        <p><code>SHIFT+ZZ</code></p>
      </td>
      <td>
            <p>Another way to Save and Exit Vi</p>
      </td>
    </tr>

    </tr>
      <tr>
      <td>
        <p><code>wq!</code></p>
      </td>
      <td>
            <p>Forces save on read only files, and exits</p>
      </td>
    </tr>    
    </tbody>
</table>
</div>

## Misc Vi Commands

### Vi Undo Command

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>U</code></p>
      </td>
      <td>
            <p>Press <code>SHIFT+U</code> to undo in Vi</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

### Vi Undo All

Vi undo all since last write.

<div class="mobile-side-scroller">
<table>
  <thead>
    <tr>
      <th>Vi Command</th>
      <th>Description</th>
    </tr>
  </thead>
      <tbody>
      <tr>
      <td>
        <p><code>:+X+!</code></p>
      </td>
      <td>
            <p>Undo everything since last write</p>
      </td>
    </tr>
    </tbody>
</table>
</div>

## Vi Show File Name

<code>SHIFT+G</code> shows the file name, number of lines and current position.

## Vi Multipliers

Almost every command in Vi can leverage multipliers, typically it's a case of prefixing the command with a numnber. Example: <code>10W</code> would move 10 words to the right.

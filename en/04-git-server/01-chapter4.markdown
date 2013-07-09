#The third use case# 

## Outline ##

One of the best things about Git is that
it is easy for several people to work on the same project at the
same time.  One the other hand, one of the worst things about
Git is that it is easy for several people working on the same
project to really mess things up.

In this chapter we walk through a use case that simulates people
editing the same file at the same time.  We'll see what goes wrong
and how to fix it.

Here are the steps:

1.   We'll use the Github web interface to create a new file.

if you have several people working on the same project, it is easy


## Adding files on Github ##

Using the Github we interface, you can create and edit files on the
remote repository without cloning a local copy.

If you want to follow along with this example, create a new project
or choose an existing project, and clone a local copy.  Now you have
two copies of the repo, one on GitHub and one on your computer.

Go to the project home page on GitHub.  Above the list of files you
should see an icon that looks like a sheet of paper with a folded corner
and a big plus sign.  Press it to add a new file to the project.

For my example, I'll call the file `delete_me.txt`, to remind
me to delete the file when I am done.

Type a few lines of text in the edit window.  Here's mine:



## Editing on GitHub ##

The project home page shows the name and project description, a list
of files, and the contents of README.md.

For a new project, README.md is usually the only file in the list.  If
you click on the blue filename, you will see the detail page for this
file.  Click "History" to see a list of commits that have modified
this file.  Click "Blame" to see history information in a different
format.  And click "Edit" to... you guessed it... edit the file.

The web editor on GitHub is useful for making small changes, but it is
not the most common way of modifying a repo.  And it is not
particularly safe.  If you are making substantial changes and want to
minimize the chance of losing work, I don't recommend using the web
editor.

Anyway, if you want to add more information to README.md, you can.  By
the way, "md" stands for Markdown, which is a markup language you can
use to format the contents of README.md.  This book is written in
Markdown.

When you are done editing, you can add comments in the "Commit
summary" and "Extended description" fields, and then hit "Commit
Changes".  The commit summary usually describes what was changed; the
description explains why.


#Pull#

#Merge#


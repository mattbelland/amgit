#The third use case#

## Outline ##

One of the best things about Git is that
it is easy for several people to work on the same project at the
same time.  One the other hand, one of the worst things about
Git is that it is easy for several people working on the same
project to really mess things up.

In this chapter we walk through a use case that simulates people
editing the same file at the same time.  We'll see what can go wrong
and how to fix it.

Here are the steps:

1. Use the Github web interface to create a new file in the remote repo.
2. Pull the change from the remote to the local repo.
3. Edit the file on both the local and remote repo and then merge the changes automatically.
4. Make simultaneous edits that can't be merged automatically.
5. Resolve the conflict. 
6. Delete the file.

If you want to follow along with this example, create a new project
or choose an existing project, and clone a local copy.  Now you have
two copies of the repo, one on GitHub and one on your computer.



## Adding files on Github ##

Using the Github web interface, you can create and edit files on the
remote repository.

Go to the project home page on GitHub.  Above the list of files you
should see an icon that looks like a sheet of paper with a folded corner
and a big plus sign.  Press it to add a new file to the project.

For my example, I'll call the file `delete_me.txt`, to remind
me to delete the file when I am done.

Type a few lines of text in the edit window.  Here's mine:

    Shopping list:
    1. Milk
    2. Chocolate frosted sugar bombs

Below the edit window you'll see the Commit summary, which GitHub
has filled in, and the Extended Description, which you can fill
in.

Press "Commit new file".  GitHub takes you back to the project home
page where you should see the new file in the file list.


## Pull the new file ##

At this point the new file is in the remote repo (on GitHub) but
not in your local copy.  To pull it into the local copy, open
a terminal, `cd` into the local repo, and type:

    $ git pull

Git responds with the usual babble:

    remote: Counting objects: 4, done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 3 (delta 1), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From https://github.com/AllenDowney/clink
       c2a760a..6c851d1  master     -> origin/master
    Updating c2a760a..6c851d1
    Fast-forward
     delete_me.txt | 3 +++
     1 file changed, 3 insertions(+)
     create mode 100644 delete_me.txt

The upshot is that the changes in the remote repo have been copied
into the local repo.

Before we go on, run 

    $ git status

To make sure there are no changes in the local repo that are not
in the remote.  You should get

    # On branch master
    nothing to commit, working directory clean



## Editing on GitHub ##

Back on GitHub, go to the project home page and click `delete_me.txt`,
or whatever you named the file
you just created.
 
You should get the detail page for the
file.  Click "History" to see a list of commits that have modified
this file.  Click "Blame" to see history information in a different
format.  And click "Edit" to... you guessed it... edit the file.

The web editor on GitHub is useful for making small changes, but it is
not the most common way of modifying a repo.  And it is not
particularly safe.  If you are making substantial changes and want to
minimize the chance of losing work, I don't recommend using the web
editor.

Anyway, go ahead and change one line of the file.  For example,
I changed "Milk" to "Skim milk."

Press "Commit changes."  Now, again, there is a commit in the remote
repo that is not in the local copy.  But don't pull it yet!


## Editing the local copy ##

Go back to your local copy and use the editor of your choice to edit
the same file, but don't edit the same line.  In my example, I added
a third item, "Kale", to the shopping list.

Now that you have changed a working file, you can stage it by adding
the file:

    $ git add delete_me.txt

Then commit the change.

    $ git commit -m "Adding kale to list"
    [master 7061131] Adding kale to list
     1 file changed, 1 insertion(+)

Or you could add and commit the change at the same time:

    $ git commit -am "Adding kale to list"

Now you have a commit in the local repo that is not in the remote
repo.  Normally you would push the change to the remote, but here's
what happens if you try:

    $ git push origin master
    To https://github.com/AllenDowney/clink
     ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'https://github.com/AllenDowney/clink'
    hint: Updates were rejected because the tip of your current branch is behind
    hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
    hint: before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

As the error message says, your push was rejected because your local
repo is "behind" its remote.  That is, there is a commit in the remote
that is not in the local repo.  You have to pull the change from the
remote before you can push your change.


## Push you, pull me ##

So let's do what we're told:

    $ git pull

This time git opens an editor and asks you to provide a message
explaining "why this merge is necessary."  It provides a default message
which you can use.  Apparently "because you told me to" is not
sufficient.

When you close the editor, Git reports:

    From https://github.com/AllenDowney/clink
       6c851d1..019c5db  master     -> origin/master
    Auto-merging delete_me.txt
    Merge made by the 'recursive' strategy.
     delete_me.txt | 2 +-
     1 file changed, 1 insertion(+), 1 deletion(-)

Which indicates that Git has automatically merged the two versions of
the file.  

Open the file.  Or if you had the file open in an editor, reload
it to see the change.  If things went according to plan, you
should see:

    Shopping list:
    1. Skim milk
    2. Chocolate frosted sugar bombs
    3. Kale

Now the local copy contains both changes (adding "Skim" and "Kale").

But we're not done because the local changes have not been pushed
to the remote yet.  If you run

    $ git push origin master

it should succeed.  Now both repos have the same set of commits,
so neither is behind the other.  They are back in sync.


## Merge conflicts ##

The previous example was one of the good cases where Git is able
to merge changes automatically.  If two people modify the same file,
Git can _usually_ merge them unless there are conflicting changes
to the same part of the file.

To see how that goes, try the following:

1.  Use the web interface to change "Chocolate frosted sugar bombs"
to "Organic shredded quinoa", and commit the change.
2.  In your local repo, change "Chocolate frosted sugar bombs"
to "Chocolate frosted sugar bombs, with marshmallows".  But don't
commit the change yet.
3.  In the local repo, run `git pull`.

You should see a message like this:

    From https://github.com/AllenDowney/clink
       3fede59..dacb3ff  master     -> origin/master
    Updating 3fede59..dacb3ff
    error: Your local changes to the following files would be overwritten by merge:
	delete_me.txt
    Please, commit your changes or stash them before you can merge.

Git is letting you know that if you pull the change from the repo it
will clobber the change you made in a working file.

It suggests you should either commit or stash the local change.
Since we don't know about "stash" yet, let's commit.

    $ git commit -am "Adding marshmallows"

And try pulling again:

    $ git pull

Git reports

    Auto-merging delete_me.txt
    CONFLICT (content): Merge conflict in delete_me.txt
    Automatic merge failed; fix conflicts and then commit the result.

As expected, Git can't merge the two files because there are two
changes to the same line and Git doesn't know which to keep,
the quinoa or the marshmallows.

If you run `git status`, you will see which file or files are
in conflict.

If you edit the conflicted file, you'll see something like this:

    Shopping list:
    1. Skim milk
    <<<<<<< HEAD
    2. Chocolate frosted sugar bombs with marshmallows
    =======
    2. Organic shredded quinoa
    >>>>>>> dacb3ffc4370b9f6983e1db4101ae738bbcafa36
    3. Kale

The file contains both versions of the conflicted line, and 
markers
indicating the source of the conflicting changes.  `HEAD` refers
to the most recent commit in the local repo.  The long
string of letters and numbers, starting with `dacb`, is the
unique identifier of the commit on the remote.

You can resolve the conflict by editing the file directly, choosing
which version to keep (the marshmallows, of course), and deleting
the conflict markers.

Or if you run

    $ git mergetool

Git opens the conflicted file using one of many file merge tools,
special editors that display conflicts in a readable form, provides
a GUI for choosing which version to keep, and writes the merged
file.

Once you have saved the merged file, add and commit the change,
and push it to origin:

    $ git commit -am "Merging delete_me.txt"
    $ git push origin master


## Deleting files ##

I called the file `delete_me.txt` for a reason.  Here's how to
delete a file:

    $ git rm delete_me.txt
    rm 'delete_me.txt'

And then, as usual, commit and push the change:

    $ git commit -am "Deleting delete_me.txt"
    [master 6554cbb] Deleting delete_me.txt
     1 file changed, 4 deletions(-)
     delete mode 100644 delete_me.txt

    $ git push origin master

And, we're done.

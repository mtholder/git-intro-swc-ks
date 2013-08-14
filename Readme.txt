Git Introduction
================

MTH notes on talk for software carpentry

Tell git that you have a directory that should be "versioned"

    $ git init
 
 If you are one a computer account that has not used git before
 then you should probably introduce yourself to git. This information
 will be associated with the changes that you make to files later:

    $ git config --global user.name "Mark T. Holder"
    $ git config --global user.email "mtholder@gmail.com"

Because working with git will involve making notes about your changes
you should probably go ahead and register your text editor with git.
The following lines tell git how to call your text editor in a way that 
will pause git until you close your editing session. The commands differ
a bit from editor to editor.  

On Windows using Notepad++ in the standard
location for a 64-bit machine, you would use:

    $ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

(thanks to
http://stackoverflow.com/questions/1634161/how-do-i-use-notepad-or-other-with-msysgit/2486342#2486342 
for that tip)

On Mac, with TextWrangler if you installed TextWrangler's command line tools
then you should have an "edit" command. So you can use the git command:

    $  git config --global core.editor "edit -w"


Let's make a change, save it to our file system, and then save it into git.

Open a file "dummy.txt" and add a few lines of any text to it.  

    $ echo 'hi, there' > dummy.txt
    $ echo 'this is a second line' >> dummy.txt
    $ echo 'note that I have to use >> to append to a file so that I do not overwrite previous content' >> dummy.txt

You save the file to your filesystem in the normal way. To tell git that 
you have made some changes that you would like to have versioned in the 
repository, you have to "add" the set of changes to git:

First note that if you do *not* add the file, then git lists the file
as an "untracked" file:

    $ git status

git status is very useful. It let's you see how git views the current
state of the directory that correspond to your repository.
 

To add a set of changes run:

    $ git add dummy.txt
 

Now if you repeat the status check:

    $ git status

you should see the new "dummy.txt" file are listed as the "Changes to be 
committed." Often when we want to flag a set of changes as constituting
a version of our software. That is why there are two steps involved in saving
a set of change. We have done the first step. Adding a file puts it on the 
"stage". To complete the transaction and make a new version of our repository
we can simply run:

   $ git commit

This will drop you enter your text editor so that you can write a message
explaining what you have done.

If you did not correctly configure your favorite text editor (see the steps
above about using the git config commands), then you'll probably 
be dropped into vim which is a great UNIX text editor. If you want to learn 
vim you should do that when you are not in the process of learning git. To exit 
vim simply type :q! then return (that is a colon, a lowercase q, and exclamation 
point and a return).

Commit messages are very important for telling others (and reminding yourself)
what you were trying to accomplish with a set of changes. The "Best Practices"
for a commit message are:
  * a short (<72 chars) summary.
  * a blank line
  * a description of the commit wrapped such that no line is longer than 72 characters.

When you are done writing the message, you can save and close it. On some
editors, you have to quit the editor session to return control to git.

If the commit message was received then the commit should succeed. You should see
a message with something like:

    [master some cryptic string that matches [0-9a-d]+ here]
    1 file changed, 3 insertions(+)
    create mode 100644 dummy.txt

To let you know that you've added changes to one file.

Understanding git
=================
What just happened? Your working directory has the current code (just a dummy.txt file
with whatever text you put in it).  Think of this as a snapshot of your code.  Git
stores snapshots.  Everytime you commit you are adding a new snapshot to git's 
repository. Git's repository is just a database of snapshots and information about
the ancestry of each snapshot. In addition to looking at the files currently stored
in your directory and previous commits stored in the repository, git also keeps track
of an intermediate staging area (often referred to as the index in git documentation).

"git add" copies file contents from your working directory into the staging area. Git
commit copies file contents from the staging area into the repository and stores the
necessary information about the commit. In particular, it keeps track of the parent of 
the commit and your commit message.  The parent is simply the name of a previously 
committed snapshot. In git documentaion, you'll often see people refer to the "HEAD."
The HEAD is the identifier for the snapshot that will be the parent of you next
commit. Try running:

    $ echo 'a new last line' >> dummy.txt
    $ git status
    $ git diff
    $ git add dummy.txt
    $ git status
    $ git commit -m 'Added a silly new last line'
    $ git status
    $ git log
 
Does the output make sense? There are two new commands here:

  * diff shows you the difference between your working directory and the snapshot in the staging area.
  * log shows you the commit messages for all of snapshots that were stored 
      in the history of the current HEAD.
 
We don't see "parent" snapshot in the output, but git stores it. It is able
to present the full history of your work because it traverses the snaphsots
from the current HEAD back until the intial point at which you added the
directory to version control (the initial commit has no parent).

Seeing the history of notes that we've made in commit messages is cute, but
not terribly useful. Fortunately, we can navigate the ancestry graph to 
restore any snapshot that we are interested in.  The command for this is 
git checkout, and it takes an argument: the name of the snapshot that you
want to restore:



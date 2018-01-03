---
title: Tracking Changes
teaching: 20
exercises: 0
questions:
- "How do I record changes in Git?"
- "How do I check the status of my version control repository?"
- "How do I record notes about what changes I made and why?"
objectives:
- "Go through the modify-add-commit cycle for one or more files."
- "Explain where information is stored at each stage of that cycle."
- "Distinguish between descriptive and non-descriptive commit messages."
keypoints:
- "`git status` shows the status of a repository."
- "Files can be stored in a project's working directory (which users see), the staging area (where the next commit is being built up) and the local repository (where commits are permanently recorded)."
- "`git add` puts files in the staging area."
- "`git commit` saves the staged content as a new commit in the local repository."
- "Always write a log message when committing changes."
---

First let's make sure we're still in the right directory.
You should be in the `inflammation` directory.

~~~
$ pwd
~~~
{: .bash}

If you are still in `infection` navigate back up to `inflammation`

~~~
$ cd ..
~~~
{: .bash}

Let's create a file called `project.txt` that contains some notes
about what we plan to do in this project.
We'll use `nano` to edit the file but you can use whatever editor you like.

In particular, this does not have to be the `core.editor` you set globally earlier. But remember, the bash command to create or edit a new file will depend on the editor you choose (it might not be `nano`). For a refresher on text editors, check out ["Which Editor?"](https://swcarpentry.github.io/shell-novice/03-create/) in [The Unix Shell](https://swcarpentry.github.io/shell-novice/) lesson.

~~~
$ nano project.txt
~~~
{: .bash}

Type the text below into the `project.txt` file:

~~~
Some initial data analysis to identify how inflammation changes over time after surgery.
~~~
{: .output}

`project.txt` now contains a single line, which we can see by running:

~~~
$ ls
~~~
{: .bash}

~~~
project.txt
~~~
{: .output}

~~~
$ cat project.txt
~~~
{: .bash}

~~~
Some initial data analysis to identify how inflammation changes over time after surgery.
~~~
{: .output}

Now, if we check the status of our project again, Git tells us that it's noticed the new file:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	project.txt

nothing added to commit but untracked files present (use "git add" to track)
~~~
{: .output}

The "untracked files" message means that there's a file in the directory
that Git isn't keeping track of.
We can tell Git to track a file using `git add`:

~~~
$ git add project.txt
~~~
{: .bash}

and then check that the right thing happened:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   project.txt

~~~
{: .output}

Git now knows that it's supposed to keep track of `project.txt`,
but it hasn't recorded these changes as a commit yet.
To get it to do that,
we need to run one more command:

~~~
$ git commit -m "Start notes on the patient inflammation project"
~~~
{: .bash}

~~~
[[master (root-commit) 40b8a94] Start notes on the patient inflammation project
 1 file changed, 1 insertion(+)
 create mode 100644 project.txt

~~~
{: .output}

When we run `git commit`,
Git takes everything we have told it to save by using `git add`
and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit]({{ page.root }}/reference/#commit)
(or [revision]({{ page.root }}/reference/#revision)) and its short identifier is `f22b25e`
(Your commit may have another identifier.)

We use the `-m` flag (for "message")
to record a short, descriptive, and specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option,
Git will launch `nano` (or whatever other editor we configured as `core.editor`)
so that we can write a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) summary of
changes made in the commit.  If you want to go into more detail, add
a blank line between the summary line and your additional notes. They are usually written in the present tense.

If we run `git status` now:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
nothing to commit, working tree clean

~~~
{: .output}

it tells us everything is up to date.
If we want to know what we've done recently,
we can ask Git to show us the project's history using `git log`:

~~~
$ git log
~~~
{: .bash}

~~~
commit 40b8a9400329ed7324116cc9392dc8e4842d8501 (HEAD -> master)
Author: Jane Smith <jane.smith@university.ac.uk>
Date:   Wed Jan 3 10:34:07 2018 +0000

    Start notes on the patient inflammation project

~~~
{: .output}

`git log` lists all commits  made to a repository in reverse chronological order.
The listing for each commit includes
the commit's full identifier
(which starts with the same characters as
the short identifier printed by the `git commit` command earlier),
the commit's author,
when it was created,
and the log message Git was given when the commit was created.

> ## Where Are My Changes?
>
> If we run `ls` at this point, we will still see just one file called `project.txt`.
> That's because Git saves information about files' history
> in the special `.git` directory mentioned earlier
> so that our filesystem doesn't become cluttered
> (and so that we can't accidentally edit or delete an old version).
{: .callout}

Now suppose Jane adds more information to the file.
(Again, we'll edit with `nano` and then `cat` the file to show its contents;
you may use a different editor, and don't need to `cat`.)

~~~
$ nano project.txt
$ cat project.txt
~~~
{: .bash}

~~~
Some initial data analysis to identify how inflammation changes over time after surgery.
Jane is a Data Scientist and Samit is a statistician. We'll need to determine
who is responsible for what in this project.
~~~
{: .output}

When we run `git status` now,
it tells us that a file it already knows about has been modified:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   project.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

The last line is the key phrase:
"no changes added to commit".
We have changed this file,
but we haven't told Git we will want to save those changes
(which we do with `git add`)
nor have we saved them (which we do with `git commit`).
So let's do that now. It is good practice to always review
our changes before saving them. We do this using `git diff`.
This shows us the differences between the current state
of the file and the most recently saved version:

~~~
$ git diff
~~~
{: .bash}

~~~
diff --git a/project.txt b/project.txt
index 17e2b79..2485f1d 100644
--- a/project.txt
+++ b/project.txt
@@ -1 +1,3 @@
 Some initial data analysis to identify how inflammation changes over time after surgery.
+Jane is a Data Scientist and Samit is a statistician. We'll need to determine
+who is responsible for what in this project.
~~~
{: .output}

The output is cryptic because
it is actually a series of commands for tools like editors and `patch`
telling them how to reconstruct one file given the other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix `diff` command
    comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file
    Git is comparing;
    `17e2b79` and `2485f1d` are unique computer-generated labels for those versions.
3.  The third and fourth lines once again show the name of the file being changed.
4.  The remaining lines are the most interesting, they show us the actual differences
    and the lines on which they occur.
    In particular,
    the `+` marker in the first column shows where we added new lines.

After reviewing our change, it's time to commit it:

~~~
$ git commit -m "Add note about project responsibilities"
$ git status
~~~
{: .bash}

~~~
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   project.txt

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}

Whoops:
Git won't commit because we didn't use `git add` first.
Let's fix that:

~~~
$ git add project.txt
$ git commit -m "Add note about project responsibilities"
~~~
{: .bash}

~~~
[master 71ed26a] Add note about project responsibilities
 1 file changed, 2 insertions(+)

~~~
{: .output}

Git insists that we add files to the set we want to commit
before actually committing anything. This allows us to commit our
changes in stages and capture changes in logical portions rather than
only large batches.
For example,
suppose we're adding a few citations to relevant research to our thesis.
We might want to commit those additions,
and the corresponding bibliography entries,
but *not* commit some of our work drafting the conclusion
(which we haven't finished yet).

To allow for this,
Git has a special *staging area*
where it keeps track of things that have been added to
the current [changeset]({{ page.root }}/reference/#changeset)
but not yet committed.

> ## Staging Area
>
> If you think of Git as taking snapshots of changes over the life of a project,
> `git add` specifies *what* will go in a snapshot
> (putting things in the staging area),
> and `git commit` then *actually takes* the snapshot, and
> makes a permanent record of it (as a commit).
> If you don't have anything staged when you type `git commit`,
> Git will prompt you to use `git commit -a` or `git commit --all`,
> which is kind of like gathering *everyone* for the picture!
> However, it's almost always better to
> explicitly add things to the staging area, because you might
> commit changes you forgot you made. (Going back to snapshots,
> you might get the extra with incomplete makeup walking on
> the stage for the snapshot because you used `-a`!)
> Try to stage things manually,
> or you might find yourself searching for "git undo commit" more
> than you would like.
{: .callout}

![The Git Staging Area](../fig/git-staging-area.png)

Let's watch as our changes to a file move from our editor
to the staging area
and into long-term storage.
First,
we'll add another line to the file:

~~~
$ nano project.txt
$ cat project.txt
~~~
{: .bash}

~~~
Some initial data analysis to identify how inflammation changes over time after surgery.
Jane is a Data Scientist and Samit is a statistician. We'll need to determine
who is responsible for what in this project.
We may need to bring a third person with Python programming skills into the project.
~~~
{: .output}

~~~
$ git diff
~~~
{: .bash}

~~~
diff --git a/project.txt b/project.txt
index 2485f1d..3f80947 100644
--- a/project.txt
+++ b/project.txt
@@ -1,3 +1,4 @@
 Some initial data analysis to identify how inflammation changes over time after surgery.
 Jane is a Data Scientist and Samit is a statistician. We'll need to determine
 who is responsible for what in this project.
+We may need to bring a third person with Python programming skills into the project.
~~~
{: .output}

So far, so good:
we've added one line to the end of the file
(shown with a `+` in the first column).
Now let's put that change in the staging area
and see what `git diff` reports:

~~~
$ git add project.txt
$ git diff
~~~
{: .bash}

There is no output:
as far as Git can tell,
there's no difference between what it's been asked to save permanently
and what's currently in the directory.
However,
if we do this:

~~~
$ git diff --staged
~~~
{: .bash}

~~~
diff --git a/project.txt b/project.txt
index 2485f1d..3f80947 100644
--- a/project.txt
+++ b/project.txt
@@ -1,3 +1,4 @@
 Some initial data analysis to identify how inflammation changes over time after surgery.
 Jane is a Data Scientist and Samit is a statistician. We'll need to determine
 who is responsible for what in this project.
+We may need to bring a third person with Python programming skills into the project.
~~~
{: .output}

it shows us the difference between
the last committed change
and what's in the staging area.
Let's save our changes:

~~~
$ git commit -m "Add note about extra team members"
~~~
{: .bash}

~~~
[master 005937f] Add note about extra team members
 1 file changed, 1 insertion(+)
~~~
{: .output}

check our status:

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
nothing to commit, working directory clean
~~~
{: .output}

and look at the history of what we've done so far:

~~~
$ git log
~~~
{: .bash}

~~~
commit 63f6bc6039c12ce897cf3f02bda1e95fd30fef58 (HEAD -> master)
Author: Jane Smith <jane.smith@university.ac.uk>
Date:   Wed Jan 3 10:42:09 2018 +0000

    Add note about extra team members

commit 71ed26a8f840d2929f5120327ea0accd55074070
Author: Jane Smith <jane.smith@university.ac.uk>
Date:   Wed Jan 3 10:39:19 2018 +0000

    Add note about project responsibilities

commit 40b8a9400329ed7324116cc9392dc8e4842d8501
Author: Jane Smith <jane.smith@university.ac.uk>
Date:   Wed Jan 3 10:34:07 2018 +0000

    Start notes on the patient inflammation project
~~~
{: .output}

> ## Word-based diffing
>
> Sometimes, e.g. in the case of the text documents a line-wise
> diff is too coarse. That is where the `--color-words` option of
> `git diff` comes in very useful as it highlights the changed
> words using colors.
{: .callout}

> ## Paging the Log
>
> When the output of `git log` is too long to fit in your screen,
> `git` uses a program to split it into pages of the size of your screen.
> When this "pager" is called, you will notice that the last line in your
> screen is a `:`, instead of your usual prompt.
>
> *   To get out of the pager, press `q`.
> *   To move to the next page, press the space bar.
> *   To search for `some_word` in all pages, type `/some_word`
>     and navigate through matches pressing `n`.
{: .callout}

> ## Limit Log Size
>
> To avoid having `git log` cover your entire terminal screen, you can limit the
> number of commits that Git lists by using `-N`, where `N` is the number of
> commits that you want to view. For example, if you only want information from
> the last commit you can use:
>
> ~~~
> $ git log -1
> ~~~
> {: .bash}
>
> ~~~
> commit 63f6bc6039c12ce897cf3f02bda1e95fd30fef58 (HEAD -> master)
> Author: Jane Smith <jane.smith@university.ac.uk>
> Date:   Wed Jan 3 10:42:09 2018 +0000
>
>    Add note about extra team members
> ~~~
> {: .output}
>
> You can also reduce the quantity of information using the
> `--oneline` option:
>
> ~~~
> $ git log --oneline
> ~~~
> {: .bash}
> ~~~
> 63f6bc6 (HEAD -> master) Add note about extra team members
> 71ed26a Add note about project responsibilities
> 40b8a94 Start notes on the patient inflammation project
> ~~~
> {: .output}
>
> You can also combine the `--oneline` options with others. One useful
> combination is:
>
> ~~~
> $ git log --oneline --graph --all --decorate
> ~~~
> {: .bash}
> ~~~
> * 63f6bc6 (HEAD -> master) Add note about extra team members
> * 71ed26a Add note about project responsibilities
> * 40b8a94 Start notes on the patient inflammation project
> ~~~
> {: .output}
{: .callout}

> ## Directories
>
> Two important facts you should know about directories in Git.
>
> 1. Git does not track directories on their own, only files within them.
> Try it for yourself:
>
> ~~~
> $ mkdir directory
> $ git status
> $ git add directory
> $ git status
> ~~~
> {: .bash}
>
> Note, our newly created empty directory `directory` does not appear in
> the list of untracked files even if we explicitly add it (_via_ `git add`) to our
> repository. This is the reason why you will sometimes see `.gitkeep` files
> in otherwise empty directories. Unlike `.gitignore`, these files are not special
> and their sole purpose is to populate a directory so that Git adds it to
> the repository. In fact, you can name such files anything you like.
>
> {:start="2"}
> 2. If you create a directory in your Git repository and populate it with files,
> you can add all files in the directory at once by:
>
> ~~~
> git add <directory-with-files>
> ~~~
> {: .bash}
>
{: .callout}

To recap, when we want to add changes to our repository,
we first need to add the changed files to the staging area
(`git add`) and then commit the staged changes to the
repository (`git commit`):

![The Git Commit Workflow](../fig/git-committing.png)

> ## Choosing a Commit Message
>
> Which of the following commit messages would be most appropriate for the
> last commit made to `project.txt`?
>
> 1. "Changes"
> 2. "Added line 'We discussed who might be responsible for what' to project.txt"
> 3. "Add note about project responsibilities"
>
> > ## Solution
> > Answer 1 is not descriptive enough,
> > and answer 2 is too descriptive and redundant,
> > but answer 3 is good: short but descriptive.
> {: .solution}
{: .challenge}

> ## Committing Changes to Git
>
> Which command(s) below would save the changes of `myfile.txt`
> to my local Git repository?
>
> 1. `$ git commit -m "my recent changes"`
>
> 2. `$ git init myfile.txt`
>    `$ git commit -m "my recent changes"`
>
> 3. `$ git add myfile.txt`
>    `$ git commit -m "my recent changes"`
>
> 4. `$ git commit -m myfile.txt "my recent changes"`
>
> > ## Solution
> >
> > 1. Would only create a commit if files have already been staged.
> > 2. Would try to create a new repository.
> > 3. Is correct: first add the file to the staging area, then commit.
> > 4. Would try to commit a file "my recent changes" with the message myfile.txt.
> {: .solution}
{: .challenge}

> ## Committing Multiple Files
>
> The staging area can hold changes from any number of files
> that you want to commit as a single snapshot.
>
> 1. Add some text to `project.txt` noting your decision
> on what a third tem member might be resposnible for.
> 2. Create a new file `analysis.txt` with your initial thoughts
> about what data would be useful for your analysis project
> 3. Add changes from both files to the staging area,
> and commit those changes.
>
> > ## Solution
> >
> > First we make our changes to the `project.txt` and `analysis.txt` files:
> > ~~~
> > $ nano project.txt
> > $ cat project.txt
> > ~~~
> > {: .bash}
> > ~~~
> > The third team member needs to be competent in both Python and R. They
> > also need to be familiar with matplotlib and ggplot
> > ~~~
> > {: .output}
> > ~~~
> > $ nano analysis.txt
> > $ cat analysis.txt
> > ~~~
> > {: .bash}
> > ~~~
> > We need data about patients, wards, dates, temperatures and inflammation size
> > ~~~
> > {: .output}
> > Now you can add both files to the staging area. We can do that in one line:
> >
> > ~~~
> > $ git add project.txt analysis.txt
> > ~~~
> > {: .bash}
> > Or with multiple commands:
> > ~~~
> > $ git add project.txt
> > $ git add analysis.txt
> > ~~~
> > {: .bash}
> > Now the files are ready to commit. You can check that using `git status`. If you are ready to commit use:
> > ~~~
> > $ git commit -m "Add note about what data would be useful"
> > ~~~
> > {: .bash}
> > ~~~
> > [master a264d5c] Add note about what data would be useful
> > 2 files changed, 3 insertions(+)
> > create mode 100644 analysis.txt
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

> ## Author and Committer
>
> For each of the commits you have done, Git stored your name twice.
> You are named as the author and as the committer. You can observe
> that by telling Git to show you more information about your last
> commits:
>
> ~~~
> $ git log --format=full
> ~~~
> {: .bash}
>
> When committing you can name someone else as the author:
>
> ~~~
> $ git commit --author="Samit Patel <samit.patel@university.ac.uk>"
> ~~~
> {: .bash}
>
> Create a new repository and create two commits: one without the
> `--author` option and one by naming a colleague of yours as the
> author. Run `git log` and `git log --format=full`. Think about ways
> how that can allow you to collaborate with your colleagues.
>
> > ## Solution
> >
> > ~~~
> > $ git add me.txt
> > $ git commit -m "Update Jane's bio." --author="Pete Jones <pete.jones@university.ac.uk>"
> > ~~~
> > {: .bash}
> > ~~~
> > [master 4162a51] Update Jane's bio.
> > Author: Pete Jones <pete.jones@university.ac.uk>
> > 1 file changed, 2 insertions(+), 2 deletions(-)
> >
> > $ git log --format=full
> > commit 4162a51b273ba799a9d395dd70c45d96dba4e2ff
> > Author: Frank N. Stein <pete.jones@university.ac.uk>
> > Commit: Jane Smith <jane.smith@university.ac.uk>
> >
> > Update Jane's bio.
> >
> > commit aaa3271e5e26f75f11892718e83a3e2743fab8ea
> > Author: Jane Smith <jane.smith@university.ac.uk>
> > Commit: Jane Smith <jane.smith@university.ac.uk>
> >
> > Jane's initial bio.
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

[commit-messages]: https://chris.beams.io/posts/git-commit/

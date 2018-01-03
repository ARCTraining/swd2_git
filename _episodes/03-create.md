---
title: Creating a Repository
teaching: 10
exercises: 0
questions:
- "Where does Git store information?"
objectives:
- "Create a local Git repository."
keypoints:
- "`git init` initializes a repository."
---

Once Git is configured,
we can start using it.
Let's create a directory for our work and then move into that directory:

~~~
$ mkdir inflammation
$ cd inflammation
~~~
{: .bash}

Then we tell Git to make `inflammation` a [repository]({{ page.root }}/reference/#repository)—a place where
Git can store versions of our files:

~~~
$ git init
~~~
{: .bash}

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

~~~
$ ls
~~~
{: .bash}

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `planets` called `.git`:

~~~
$ ls -a
~~~
{: .bash}

~~~
.	..	.git
~~~
{: .output}

Git stores information about the project in this special sub-directory.
If we ever delete it,
we will lose the project's history.

We can check that everything is set up correctly
by asking Git to tell us the status of our project:

~~~
$ git status
~~~
{: .bash}

If you are using a different version of git than I am, then then the exact
wording of the output might be slightly different.

~~~
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
~~~
{: .output}

> ## Places to Create Git Repositories
>
> Jane starts a new project, `infection`, related to her `inflammation` project.
> Despite Samit's concerns, ahe enters the following sequence of commands to
> create one Git repository inside another:
>
> ~~~
> $ cd                   # return to home directory
> $ mkdir inflammation   # make a new directory inflammation
> $ cd inflammation      # go into inflammation
> $ git init             # make the inflammation directory a Git repository
> $ mkdir infections     # make a sub-directory inflammation/infection
> $ cd infection         # go into inflammation/infection
> $ git init             # make the infection sub-directory a Git repository
> ~~~
> {: .bash}
>
> Why is it a bad idea to do this? (Notice here that the `inflammation` project is now also tracking the entire `infection` repository.)
> How can Jane undo her last `git init`?
>
> > ## Solution
> >
> > Git repositories can interfere with each other if they are "nested" in the
> > directory of another: the outer repository will try to version-control
> > the inner repository. Therefore, it's best to create each new Git
> > repository in a separate directory. To be sure that there is no conflicting
> > repository in the directory, check the output of `git status`. If it looks
> > like the following, you are good to go to create a new repository as shown
> > above:
> >
> > ~~~
> > $ git status
> > ~~~
> > {: .bash}
> > ~~~
> > fatal: Not a git repository (or any of the parent directories): .git
> > ~~~
> > {: .output}
> >
> > Note that we can track files in directories within a Git repository:
> >
> > ~~~
> > $ touch patient1 patient2 patient3 patient4        # create infection files
> > $ cd ..                                            # return to infection directory
> > $ ls infection                                     # list contents of the infection directory
> > $ git add infection/*                              # add all contents of inflammation/infection
> > $ git status                                       # show infection files in staging area
> > $ git commit -m "add patient files"                # commit inflammation/infection to inflammation Git repository
> > ~~~
> > {: .bash}
> >
> > Similarly, we can ignore (as discussed later) entire directories, such as the `infection` directory:
> >
> > ~~~
> > $ nano .gitignore # open the .gitignore file in the text editor to add the moons directory
> > $ cat .gitignore # if you run cat afterwards, it should look like this:
> > ~~~
> > {: .bash}
> >
> > ~~~
> > infection
> > ~~~
> > {: .output}
> >
> > To recover from this little mistake, Jane can just remove the `.git`
> > folder in the `infection` subdirectory. To do so she can run the following command from inside the `infection` directory:
> >
> > ~~~
> > $ rm -rf .git
> > ~~~
> > {: .bash}
> >
> > But be careful! Running this command in the wrong directory, will remove
> > the entire git-history of a project you might wanted to keep. Therefore, always check your current directory using the
> > command `pwd`.
> {: .solution}
{: .challenge}

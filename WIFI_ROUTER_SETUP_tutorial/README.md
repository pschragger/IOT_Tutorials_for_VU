# Router setup


## Purpose

 - Work on source control check out
 - create your own tutorial on how to setup your routers


##  Clone your personal class repo to your local system

You need to use the repo you created in the first assignment and clone it locally.

For example, I created the repo on github for this class as:
https://github.com/pschragger/VU_FALL22_IOT_CLASS

On my local machine I then move my working directory to a place I want to clone my repo with a cd command in a command or term window;
I have created a GitHub folder in my Documents folder so I change to this directory:

 cd /Users/pschragger/Documents/GitHub

And now I clone my repo using:

git clone https://github.com/pschragger/VU_FALL22_IOT_CLASS

( This assumes that you have a git package locally ). If you need to install git then follow the tutorial that matches your needs from [https://www.atlassian.com/git/tutorials/install-git](https://www.atlassian.com/git/tutorials/install-git)
Or You can use GitHUb desktop instead. Following
[installing-github-desktop](https://docs.github.com/en/desktop/installing-and-configuring-github-desktop/installing-and-authenticating-to-github-desktop/installing-github-desktop)

Once you have cloned your repo locally you need to change directory to the top level folder.  For me that would be

cd VU_FALL22_IOT_CLASS
-or-
cd /Users/pschragger/Documents/GitHub/VU_FALL22_IOT_CLASS

Next we need to create a branch to develop on.  Refer to: 
(https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

git branch Router_tutorial 
git checkout Router_tutorial

You should see a message that says: Switched to branch 'Router_tutorial'

You are now ready to create you own tutorial.

##  Start creating your own tutorial description how you will setup your router.


- create the directory

mkdir Setup_Router_Tutorial

- create a README.md in the new tutorial directory

cd Setup_Router_Tutorial/
edit README.md


You will be using a simple markuplanguage for the file.  The syntax can be found at 
[basic-writing-and-formatting-synta](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

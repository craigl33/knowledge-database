\<- [Home](home)

This is a general guide into using the version control system git. It is not meant to be a complete guide, but rather a short introduction into the logic behind git and version control. For more details see the [git documentation](https://git-scm.com/docs). Also there is a video series [here](https://www.youtube.com/watch?v=3RjQznt-8kE&list=PL4cUxeGkcC9goXbgTDQ0n_4TBzOO0ocPR). ISU also provides some information [here](https://python.iea.org/doc/getting-started/doc-gitlab-iea/sdlc.html#gitlab-on-premise-at-the-iea).

The most important commands are listed [here](Git-Cheat-Sheet).

## Why use version control?

Version control is the practice of recording changes to a file or set of files over time so that you can recall specific versions later. This is very useful for individual development, but even more so for collaborative development. It allows you to work on the same code base without interfering with each other. And any changes are tracked and can be reverted if necessary. Version control normally refers to a system called git, which is the most popular version control system and also used here.

## Setup

Git is a distributed version control system, which means that the complete repository is copied to your local machine. A git repository is the folder where all files are stored and tracked. This is why having a single project folder is important. If collaboration is not necessary, you can simply use git locally and have some benefits without the need for a remote repository. For collaboration, you need a remote repository. The most popular one is GitHub, but there are also other options like GitLab or Bitbucket and you can also self-host a git server. The IEA/RISE uses GitLab which is similar to GitHub but allows the IEA to host on an internal server (I think).

### Install git

Git needs to be installed on the local machine. At IEA this can be done via the Software Center. To check if git is installed, open a terminal and type `git --version`.

### Global settings

Once installed you can set some global settings. This only has to be done once. There are many settings, but the only one you have to set is the username and email address. This is used to identify the person who made a commit. Just use the email address as for your GitLab account. The name can be anything, so far the convention is to use your first and last name.

```
git config --global user.name "Your Name"
git config --global user.email "Your Email"
```

### Initial GitLab connection

The GitLab web application can be accessed using Single Sign On on Windows, just like the intranet or Office 365. Presumably that has worked if you have reached this page.

However, connections to GitLab repositories cannot be established using SSO, either SSH keys or HTTPS must be used. IT recommends that we connect using SSH keys, not HTTPS. Set up an SSH key as described [here](https://python.iea.org/doc/getting-started/doc-gitlab-iea/sdlc.html#gitlab-on-premise-at-the-iea) by ISU, though beware: use the command `ssh-keygen -t rsa` instead of `ssh-keygen`. Ensure you have an SSH key set up on each machine (laptop and each VM).

Once the SSH key is set up, go to the GitLab project web page and find the SSH URL from the blue `Code` dropdown button at the top, e.g. for `plexos-model-setup` the SSH URL is `git@gitlab.iea.org:iea/ems/rise/plexos-model-setup.git`. Use this URL in the git command `git remote add origin <repo_url>` on your machine/VM, e.g. `git remote add origin git@gitlab.iea.org:iea/ems/rise/plexos-model-setup.git`.

`origin` is the name of the remote repository. You can choose any name you want, but `origin` is the standard name. And in most cases, you only have one remote repository, so just stick with `origin`.

### Initialise a local repository

You may also want to initialise a local git repository. To do so, open a terminal in the project folder and type `git init`. This creates a hidden folder called `.git` in the project folder. This folder contains all the information about the repository and should not be deleted or changed manually. But you also never have to touch it.

The command `git init` is only initializing a new local repository, the remote connection and the data exchange is still missing (see below). To do that all at once with an existing repository, you can use `git clone <url>`. It creates a new local folder with the name of the repository, copies all files from the remote repository into it and also initialises a local repository with a remote connection. This is the most common way to do the setup.

## Using git

This is mainly an explanation. The most important commands are listed [here](Git-Cheat-Sheet).

Git is a command-line tool, but there are also many graphical user interfaces (GUIs) available. For example, PyCharm has a built-in git GUI. But for this guide, we will only use the command-line interface. I personally would advise against using a GUI, since it is easier to learn the commands and you have more control over what you are actually doing. So the chance of messing something up is smaller. But it is up to you.

### Basic commands (individual use and collaboration)

Those commands are used doesn't matter if you add a remote repository or not. In general the version control workflow is the following:

1. You make some changes to the code (Let's say you have change A and B)
2. With change A you are happy and want to keep it, but change B is still work in progress
3. You now add change A to the staging area
4. You create a new commit based on the staging area
5. You continue working on change B
6. If collaboration is needed, you push your changes to the remote repository

Which translates to the following commands:

```
git status # shows the current changes
git add <file> # adds a file to the staging area
git commit -m "commit message" # creates a new commit with all files in the staging area
git push origin <branch> # pushes all commits to the remote repo
git pull origin <branch> # pulls all commits from the remote repo
```

The staging area exists to allow you to create commits with only some of the changes. The staging area can also be easily reverted and nothing is written to the git history yet. While technically you can revert commits as well, it is not recommended to do so. A commit writes to the history (even with a comment) annd should be used to mark a specific state of the code. If you want to revert the changes of a commit, you should change the code back again and do another commit.

### Communication with remote repository

For collaboration you also want to communicate with a remote repository. How to create this connection was described above. The communication between the local and remote repository is then done only when you tell them to, by using the following commands:

```
git push origin <branch> # pushes all commits to the remote repo
git pull origin <branch> # pulls all commits from the remote repo
```

If you are working alone and nobody else is working on the same code base, not really anything can create problems here.

#### Merge conflicts

But if multiple people are working on the same code base, it can happen that you try to push your changes, but someone else already pushed their changes on the same code parts. This is called a merge conflict and can sometimes be a bit tricky to solve. This only happens if the changes are on the exact same lines. If you are working on different parts of the code, there is no problem and two file versions are merged without a conflict. With some communication merge conflicts can be prevented in the first place. And this is where branches come into play.

### Branches

Branches are a way to work on the same code base without interfering with each other. A branch is basically a copy of the code base. There is one main branch called `main` (sometimes also `master`) and all other branches are based on this one. If you work alone it is fine to push all commits directly on the main branch. But if you work with others, you should create a new branch for each person (or feature) you are working on. This way you can push your changes to the remote repository without interfering with the main branch. And if you are done with your feature (can be a combination of multiple commits), you can merge your branch back into the main branch. This is called a pull request and can be done on GitHub. This way you can also review the changes and discuss them with others before merging them into the main branch. While this can be done in the command line, doing this in GitHub gives you a nice overview of the changes and makes it easier to review them and to solve any potential merge conflicts.

The simplest way to switch between and create new branches is:

```
git switch <branch> # switches to an existing branch
git switch -c <branch> # creates a new branch and switches to it
```

So the workflow would than be:

1. Get the latest changes from the remote repository's main branch
2. Create a new branch
3. Make your changes
4. Commit your changes
5. Push your changes to the remote repository
6. Create a pull request on GitHub
7. Discuss and merge the pull request on GitHub
8. Update your local main branch with the latest changes from the remote repository (Your's or someone else's)

which translates in to the following commands:

1. `git pull origin main` (on master branch)
2. `git switch -c <branch>`
3. Make your changes
4. `git add <file>` and `git commit -m "commit message"` (on )
5. `git push origin <branch>` (on , creates a new branch with the same name on the remote repository)
6. Create a pull request on GitHub
7. Discuss and merge the pull request on GitHub
8. `git switch main` & `git pull origin main`

##### New environment YAML

From time to time, you may install new dependencies for the repository. It is thereforre advised to periodically updated your environment.yaml file so that any new users can install an equivalent set-up when cloning the repository. To do so, use the following command in the terminal (**making sure that the appropriate environment is active!**):

```
conda env export > environment.yml
```
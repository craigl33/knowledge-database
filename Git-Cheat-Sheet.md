<- [README](README.md)

Another cheat sheet for git can be found [here](https://education.github.com/git-cheat-sheet-education.pdf). And the full git documentation [here](https://git-scm.com/doc).


## Setup

    git init # initialize local repository
    git remote add origin <url> # connect to remote repository
    git pull origin <branch> # pull all commits from remote repository (if any)

or clone an existing repository (which does all three steps above at once)

    git clone <url> # clone an existing repository


## Basic commands

    git status # shows the current changes
    git add <file> # adds a file to the staging area
    git commit -m "commit message" # creates a new commit with all files in the staging area
    
## Branches

    git branch # shows all branches
    git switch <branch> # switches to an existing branch
    git switch -c <branch> # creates a new branch and switches to it
    git branch -d <branch> # deletes a branch

## Communication with remote repository
    
    git push origin <branch> # pushes all commits of your current branch to the remote given branch (<branch>)
    git pull origin <branch> # pulls all commits from the remote given branch (<branch>) to your current branch


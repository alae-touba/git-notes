
# table of contents
- [table of contents](#table-of-contents)
- [git cheat sheet](#git-cheat-sheet)
  - [configuration](#configuration)
    - [override work laptop config for personal projects](#override-work-laptop-config-for-personal-projects)
    - [list local git config](#list-local-git-config)
    - [disbale gpg signing locally](#disbale-gpg-signing-locally)
  - [branches](#branches)
    - [create and switch to a new branch](#create-and-switch-to-a-new-branch)
    - [rename a local branch](#rename-a-local-branch)
    - [force-delete a local branch](#force-delete-a-local-branch)
    - [delete a remote branch](#delete-a-remote-branch)
    - [show tracked remote branch](#show-tracked-remote-branch)
    - [push and set upstream](#push-and-set-upstream)
    - [list local branches](#list-local-branches)
    - [list all branches (local + remote)](#list-all-branches-local--remote)
    - [show current branch name](#show-current-branch-name)
  - [remote repositories](#remote-repositories)
    - [add a remote repository](#add-a-remote-repository)
    - [list all remotes](#list-all-remotes)
  - [commits](#commits)
    - [amend last commit message](#amend-last-commit-message)
    - [commit without gpg signing](#commit-without-gpg-signing)
  - [undoing changes](#undoing-changes)
    - [revert to an old commit range](#revert-to-an-old-commit-range)
    - [discard all uncommitted changes](#discard-all-uncommitted-changes)
    - [restore a file to last commit](#restore-a-file-to-last-commit)
    - [delete the last commit](#delete-the-last-commit)
  - [stashing](#stashing)
    - [stash tracked files with a message](#stash-tracked-files-with-a-message)
    - [stash including untracked files](#stash-including-untracked-files)
    - [list all stashes](#list-all-stashes)
    - [show details of a specific stash](#show-details-of-a-specific-stash)
    - [apply a stash](#apply-a-stash)
  - [commit inspection](#commit-inspection)
    - [show latest commit details (head)](#show-latest-commit-details-head)
    - [show parent of head commit](#show-parent-of-head-commit)
    - [show grandparent of head commit](#show-grandparent-of-head-commit)
  - [merging](#merging)
    - [abort a merge](#abort-a-merge)
    - [abort a rebase](#abort-a-rebase)
- [a typical worflow when working on a feature branch](#a-typical-worflow-when-working-on-a-feature-branch)
- [difference between `git fetch` and `git pull`](#difference-between-git-fetch-and-git-pull)
  - [git fetch (The "Check for Updates" Command)](#git-fetch-the-check-for-updates-command)
    - [What it does:](#what-it-does)
    - [example](#example)
    - [Use Cases for git fetch](#use-cases-for-git-fetch)
  - [git pull (The "Get and Integrate Updates" Command)](#git-pull-the-get-and-integrate-updates-command)
    - [What it does](#what-it-does-1)
    - [example](#example-1)
    - [Use Cases for git pull](#use-cases-for-git-pull)
- [What's a Remote Tracking Branch in Git?](#whats-a-remote-tracking-branch-in-git)
  - [Understanding Remote Tracking Branches](#understanding-remote-tracking-branches)
  - [Keep your local branch up to date](#keep-your-local-branch-up-to-date)
  - [Having more than one remote](#having-more-than-one-remote)

# git cheat sheet

## configuration

### override work laptop config for personal projects  
your work laptop likely has a **global git config** with your work email/username. to use your personal github account for a project:

```bash
git config --local user.email "alae2ba@gmail.com"
git config --local user.name "alae-touba"
```

### list local git config
```bash
git config --local --list
```

### disbale gpg signing locally
```bash
git config commit.gpgSign false
```

## branches

### create and switch to a new branch
```bash
git checkout -b <branch-name>
```

### rename a local branch
```bash
git branch -m <new-branch-name>
```

### force-delete a local branch
```bash
git branch -D <branch-name>
```

### delete a remote branch
```bash
git push origin --delete <branch-name>
```

### show tracked remote branch
```bash
git branch -vv
```

### push and set upstream
```bash
git push -u origin <branch-name>
```

### list local branches
```bash
git branch
```

### list all branches (local + remote)
```bash
git branch -a
```

### show current branch name
```bash
git branch --show-current
```

## remote repositories

### add a remote repository
```bash
git remote add origin <repo-url.git>
```

### list all remotes
```bash
git remote -v
```

## commits

### amend last commit message
```bash
git commit --amend -m "New commit message"
```

### commit without gpg signing
```bash
git commit --no-gpg-sign -m "Message"
```

## undoing changes

### revert to an old commit range
```bash
git revert --no-commit 0766c053..HEAD
git commit -m "Revert changes"
```

### discard all uncommitted changes
```bash
git reset --hard HEAD
```

### restore a file to last commit
```bash
git checkout -- <file-name>
```

### delete the last commit
```bash
git reset --hard HEAD^
```

## stashing

### stash tracked files with a message
```bash
git stash push -m "Stash message"
```

### stash including untracked files
```bash
git stash push -u -m "Stash message"
```

### list all stashes
```bash
git stash list
```

### show details of a specific stash
```bash
git stash show stash@{<id>}
```

### apply a stash
```bash
git stash apply stash@{<id>}
```

## commit inspection

### show latest commit details (head)
```bash
git show HEAD
```

### show parent of head commit
```bash
git show HEAD^
```

### show grandparent of head commit
```bash
git show HEAD^^
```

## merging

### abort a merge
```bash
git merge --abort
```

### abort a rebase
```bash
git rebase --abort
```


# a typical worflow when working on a feature branch

Here's the complete workflow including stash for when you need to switch branches with uncommitted changes:

1. Start your feature branch from up-to-date dev:

    ```bash
    git checkout dev
    git pull
    git checkout -b feature-branch

    ```

2. Work on your feature, making commits as needed:

    ```bash
    git add .
    git commit -m "feat: add login form"
    git commit -m "fix: improve validation"
    # ... more commits as you work
    ```

3. When you need to get latest dev changes but have uncommitted work:

    ```bash
    # Save your uncommitted changes
    git stash save "WIP: login feature styling"

    # Get latest dev changes
    git checkout dev
    git pull
    git checkout feature-branch
    git rebase dev    # This puts your commits on top of latest dev changes

    # Bring back your uncommitted changes
    git stash pop     # You might need to resolve conflicts here

    ```

4. Test that everything still works
5. Finally, merge your feature into dev:

    ```bash
    git checkout dev
    git merge feature-branch
    git push

    ```




# difference between `git fetch` and `git pull`

git fetch: **Retrieves** changes from a remote repository but **does not integrate** them into your local working branch. It's like downloading updates but not installing them yet.

git pull: **Retrieves** changes from a remote repository and **immediately integrates** them into your current local working branch. It's like downloading and installing updates in one go.


## git fetch (The "Check for Updates" Command)

### What it does:

- Connects to the remote repository (usually `origin` by default).
- Downloads new commits, branches, and tags from the remote.
- **Updates your remote-tracking branches**. These are special branches in your local repository that mirror the branches on the remote (e.g., `origin/main`, `origin/develop`). Think of them as bookmarks pointing to the latest state of the remote branches.
- **Does NOT** change your current local branch or your working directory.

### example 

Let's say you have a local repository and the remote repository (`origin`) has been updated by someone else.

```bash
git checkout main
git fetch origin  # Fetch updates from the 'origin' remote
```

After running git fetch origin:

- Your local `main` branch remains unchanged.
- Your `origin/main` remote-tracking branch is updated to reflect the latest `main` branch on the `origin` remote.
- You can now see the changes by comparing your local `main` with `origin/main`.

```bash
git log main..origin/main  # Show commits that are in origin/main but not in your local main
git diff main origin/main  # Show the differences between your local main and origin/main
```


### Use Cases for git fetch

- **Reviewing Remote Changes**: You want to see what others have been working on before integrating their changes into your local branch. This is a safe way to check for updates without immediately modifying your working directory.
- **Checking for Updates**: You want to know if the remote repository has been updated since your last synchronization.
- **Inspecting Specific Remote Branches**: You can fetch specific branches if you're interested in changes in a particular feature branch on the remote:

    ```bash
    git fetch origin feature-branch
    ```
    This will update your `origin/feature-branch` remote-tracking branch.

- **Preparing for a Merge or Rebase**: Often, you'll git fetch first to see the latest remote changes and then decide whether to merge or rebase them into your local branch.





## git pull (The "Get and Integrate Updates" Command)
   
### What it does

- Combines two operations:
  - `git fetch`: First, it performs a `git fetch` to download the latest changes from the remote repository.
  - `git merge` (or `git rebase`): Then, it immediately merges the fetched changes from the remote-tracking branch into your current local branch. By default, it uses `git merge`.

### example

Let's say you are on your local `main` branch and you want to get the latest changes from the `origin/main` branch and integrate them into your local `main`.

```bash
git checkout main
git pull origin main  # Pull changes from 'origin/main' into your local 'main'
```

After running `git pull origin main`:
- Git performs `git fetch origin` in the background.
- Then, it performs `git merge origin/main` into your current `main` branch.
- Your local ``main` branch is now updated with the latest changes from the `origin/main`  branch.
- Your working directory is also updated to reflect the merged changes.



### Use Cases for git pull

- **Quickly Updating Your Local Branch**: When you want to synchronize your local branch with the remote branch and immediately start working with the latest changes. This is a common workflow when you are confident that you want to integrate the remote changes.
- **Starting a New Task**: Often, before starting a new task, you'll *git pull* your main development branch (like `main` or `develop`) to ensure you are building on the latest codebase.





# What's a Remote Tracking Branch in Git?

When working with Git, you'll often encounter branches that look like remotes/origin/master or origin/main. These are called remote tracking branches, and they play a crucial role in synchronizing your local work with remote repositories. But what exactly are they, and how do they work?

## Understanding Remote Tracking Branches

A remote tracking branch is essentially a local, read-only snapshot of the state of a branch on a remote repository. Think of it as a bookmark that your local Git repository uses to remember what a particular branch looked like on the remote the last time you communicated with it.

For example, remotes/origin/master (often shortened to origin/master) is a remote tracking branch that reflects the state of the master branch on the origin remote.

You can see remote tracking branches if you run:

```bash
git branch -r
```

## Keep your local branch up to date

To update your local branch with changes from the remote branch, you can:

1. **Fetch the latest changes from the remote `origin`**:

```bash
git fetch origin
```

This is a safe operation that will only update the remote tracking branches for the `origin` remote (which are `remotes/origin/*`) and will not touch your the local branches (local branches are branches that exist only in the local repository, you can see them if you run `gir branch`)  

1. **Merge or rebase the changes**

merge

```bash
git merge origin/main
```

or rebase:

```bash
git rebase origin/main
```

## Having more than one remote

You can have more than one remote in a Git repository. This is useful in various scenarios, such as collaborating with different teams, using different remote repositories for different purposes, or working with forks of a project.

Here's how you can add and manage multiple remotes in your Git repository:

1. **View Current Remotes**:
    - List the existing remotes:
        
        ```bash
        git remote -v
        ```
        
2. **Add a New Remote**:
    - Use the `git remote add` command to add a new remote. For example, to add a remote named `upstream`:
        
        ```bash
        git remote add upstream <https://github.com/another-user/another-repo.git>
        ```
        
3. **Fetch from a Specific Remote**:
    - You can fetch changes from the newly added remote:
        
        ```bash
        git fetch upstream #this will update remote tracking braches remotes/upstream/*
        ```
        
4. **Push to a Specific Remote**:
    - Push changes to a different remote, e.g., `upstream`:
        
        ```bash
        git push upstream main #push the local current branch to the main branch on the upstream remote
        ```
        
5. **Pull from a Specific Remote**:
    - Pull changes from a different remote:
        
        ```bash
        git pull upstream main
        ```
        
    
        fetch and integrate changes from the `main` branch of the `upstream` remote repository into your current branch.
        
        Its as if you executed these 2 commands instead:
        
        ```bash
        git fetch upstream
        git merge upstream/main
        ```
    
6. **Remove a Remote**:
    - If you no longer need a remote, you can remove it:
        
        ```bash
        git remote remove upstream
        ```
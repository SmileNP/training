# Udemy Git
[Link to coursework repo](https://github.com/SmileNP/GitHub-Udemy-Course)
## Introduction and Setup

I used homebrew to install SourceTree

- Use SourceTree for GUI Git control
    - SourceTree is associated with Bitbucket

## Using Git and GitHub 101

Follow this order to reduce headaches in the future (People Rarely Incinerate Read Porcupines):

1. Project 
2. Repo
3. Ignore
4. Remote
5. Push

Git has 100 mb limit per file

- Use split view for SourceTree
- SourceTree has “bookmarks” for git projects and they can be named differently from the project itself

## Common Use Cases

### Stashing

- put away changes temporarily
- can always get them back
- can be named

### Commit

- Commits have commit hashes
    - commit hashes are chained
- Branches are points to commits
- Commits are a string of changes

### Branches

Git is a great slave, but a terrible master

- Use branches liberaly
- Hanging branches are unmerged branches
- Branches are pointers to commits

### Merging

Merge early and often

- Resolving merge conflict
    - Mine: current branch
    - Theirs: the branch we’re merging in

### Revert or Reverse Commit

- Reset changes commit history to the chosen commit
- Reverse adds a new commit to the history

## Real World Use For Power Users

- Repository
    - has branches
    - has contributors
    - has organizations
    - only one owner
- Orgs
    - have repos
- Contributors
    - have repos

Everything is marketing, put in some work on the presentation of your Github Profile

### How and When to Force Push

-f is to force push from cli

force push deletes history, there is lots of protection over it,

branches can be set to be protected

### Diff stats

git diff shakey —stat ‘*.py’ → gives stats of the diff of only py files

### Cherry Picking

Pick commits and maybe only one of the files from that commit to the new commit.

### LFS Large File System

`git lfs migrate` to use large files in the repo

docs: https://git-lfs.com/

### Blame

Shows who wrote each line of code.
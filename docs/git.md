## What we have seen last week
* git concept & basic commands
* branches
* pull request & review process
* simple feature branch workflow
* ci/cd introduction
* zoom on github

## Commands recap
* `git init` Initialize a git repository
* `git add <file_name>` add a file to staging area
* `git commit -m "an explicit message"` 
* `git push origin <branch_name>` 
* `git branch <branch_name>` create a new branch
* `git checkout <branch_name>` switch to branch `<branch_name>`
* `git checkout -b <branch_name>` create and switch to new branch `<branch_name>`

## Branch & coding workflow
> **Reminder**: Don't code on master branch

### The minimal workflow, can be used when coding alone

``` mermaid
---
title: Minimal workflow
---
%%{init: { 'logLevel': 'debug', 'theme': 'base' } }%%
gitGraph
   commit
   commit
   branch develop
   checkout develop
   commit
   commit
   checkout main
   merge develop
   commit
commit
```


### A basic feature branch workflow

``` mermaid
---
title: Basic feature branch workflow
---
    %%{init: { 'logLevel': 'debug', 'theme': 'base' } }%%
    gitGraph
       commit id: "1"
       commit id: "2"
       branch feature_1
       checkout feature_1
       commit id: "3"
       checkout main
       commit id: "4"
       checkout main
       branch feature_2
       checkout feature_2
       commit id: "5"
       checkout main
       commit id: "6"
       checkout feature_1
       commit id: "7"
       checkout main
       merge feature_1 id: "customID" tag: "customTag" type: REVERSE
       checkout feature_2
       commit id: "8"
       checkout main
       commit id: "9"
```

## Best practices
### Make single-purpose & small commits
By creating small commits, it helps everyone in a team to understand what have been done.
Also, it's easier to revert a small change in case of bug.


### Share only what is necessary, add .gitignore to you repository
`.gitignore` list all the files and folder that must not be tracked

#### `.ginignore` simple example : 

```
__pycache__/
venv/
data/
download/
log.txt
any_file_you_want_to_exclude.any_extension
```

### Commit often & branch frequently
Prefer short-term branch, this will improve the traceability and highly simplify the code review process.
Try to not include large number of change in the same branch, and avoid unrelated changes.

> It's better to commit something un-perfect than nothing  

### Write detailed commit message (but short !)
When reading a commit message, anyone should be able to understand what have been done.
In general, try to explain what changed from previous code and why.

Some good examples: 

* Change number of epochs from 20 to 40
* Filter samples that contains null values
* Change unit from miles to kilometers in compute_distance method

Some bad examples:

* Update file1, file2
* A modification
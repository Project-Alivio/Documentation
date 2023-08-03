# Overview

This section will discussion basic git workflow for contributing to the application. In general when adding a new feature, you want to follow these steps: 

1. Create a new branch. 
2. Work on your feature 
3. Once you are done create a pull request to merge the branch into main, specify 2 reviewers. 

## Branches 

To make a new branch and then switch to it you can do either of these: 

```
$ git branch <new_branch_name>
$ git checkout <new_branch_name>

OR 

$ git checkout -b <new_branch_name>
```

## Pull Requests

When a feature is finished, we want to merge the code / content on the new branch back into main. Since the main branch is protected, don't attempt to directly merge the main branch with yours. Instead, push your changes to the remote and create a pull request. 

To create the PR do the following: 

1. Ensure your branch is up to date and you've pushed all the changes that you want to merge to the remote 
2. Open the Project-Alivio/alivio-prioritization page
3. At the top-bar click on the **Pull request** button. 
4. Hit **New pull request**, and add 2 reviewers. Add one older member of the team and one newer member if possible.  
5. Done!

### What to check as a reviewer

The motivation of having PRs is that everyone gets exposure to various parts of the app. This should help us all stay moderately familiar with different app components. It also helps us maintain code quality. As a reviewer look out for the following: 

- Clean code style 
- Proper commenting when necessary 
- Code correctness
- Error handling / edge cases 
- Performance considerations 

## Merge conflicts

In the event of merge conflicts do the following: 

1. Open the files with conflicts, merge the code as desired. 
2. Once code has been merged, save the files and `git add` them. 
3. Make a new "merge commit" to finalize the merge 
4. Done!

## Prettier / Precommits

We use a linter called Prettier to run pre-commit formatting of code style. When you attempt to commit code, a git hook will run which will check your code for style. 

- The purpose of this is for us to maintain a uniformly styled, easily readable repository. 
- If your code fails the linter, you'll be given a descriptive message of what to fix and the commit will be aborted. 
    - Then, once you make the changes required by the linter you can re-commit.
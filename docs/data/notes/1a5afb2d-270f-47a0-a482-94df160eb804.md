
# Overview
A commit is the state of a folder structure at a given point in time. It is a snapshot— not a diff
- There is a misconception that Git stores diffs, since we see diffs when using github, or using common commands like `git diff`. However, these diffs are dynamically generated, given 2 different commits. In fact, we can see a diff of any 2 commits. If Git stored diffs, we wouldn't be able to see a diff between 2 commits that are far apart from each other.
    - Under the hood, Git is comparing the root trees of each commit, and taking note of the paths that have different Object Ids
        - ex. if we have 2 commits and the only different is an edit made in the README, Git is able to get that diff by introspecting on the trees, and noticing that the objects representing that README file have different OIDs. When the OIDs do not match, it means they have different contents

You tell Git you want to save a snapshot of your project with `git commit` and it basically records a manifest of what all of the files in your project look like at that point
    - Most of the git commands interact with those manifests in some way
We can think about Git as a tool for storing, comparing and merging snapshots of our project
- We can get a snapshot of an individual file with `git checkout <Commit SHA> <filename>`
- A snapshot is basically a commit
- Snapshot is to a repository as screenshot is to a video, since it represents one moment of time in that video

## Ahead/Behind
- If your current branch is 3 commits *ahead* of master, it means the current branch has 3 commits that don't exist on master
- If the current branch is 3 commits *behind* master, it means there are 3 commits on master that don't exit on the current branch
- The ahead number tells you roughly how much impact the current branch will have on the base branch should it be merged.
- The behind number tells you how much work has happened on the base branch since this branch was started.
    - If the number is high, it's an indication that there will not be a clean merge. This would be a good time to merge master (or other base branch) into the current branch, which would bring the "behind" number to 0
- In the follow diagram:
    - A is 2 commits behind and 0 commits ahead of B
    - B is 0 commits behind and 2 commits ahead of A
    - C is 1 commit behind and 2 commits ahead of A
    - C is 3 commits behind and 2 commits ahead of B

![](/assets/images/2021-03-06-16-45-56.png)

## Detached State
- Occurs when we check out a commit or a remote branch, as opposed to a local branch.
    - Put another way, any ref that does not originate from your line of commits (and would thus be unable to trace any sort of history with the code you'd been working on)
- If we were to develop in detached mode then try to merge it into master, git would complain to us, because by definition, detached mode means there is no path to get back into master. (ie. there is no way to reference that feature "branch")

## Fast-Forward commit
- Occurs when we tell git to merge our feature branch into master, but git realizes that there are no 2 branches to merge, but instead the master just needs to be brought up to speed to the current feature branch. This is known as *fast-forwarding*
- effectively what is happening is Git engine is moving the current branch tip up to the target branch tip.
- If the tip of master hasn't moved since we branched off, no merge commit will be made. The HEAD will simply move to the most recent commit of the feature branch.
	- In a ff merge scenario, since the master has not changed since we branched, a "merging/melding" of branches is not relevant here.


### Forcing a FF Merge
This ff may not be desirable. Imagine we want to merge in a branch called *oauth*. This is a pretty significant feature, and we'd like to keep the branch in our history. This is a scenario where we can merge with `--no-ff`.

![240d716cd6708b64cbefb47f30773132.png](:/5edfa6a1c2a4442faff1e60705f87d32)

- ***non fast-forwarding*** (a.k.a *3 way merge*) ex. - we create a feature branch from master. We do some work on feature, then decide to merge into master. Since we had branched off from master, other feature branches have been merged into master. This means the git engine has to figure out how to *merge* the master branch (which is now different from how our feature remembers it) and our feature branch.
    - If this were a *fast-forward merge*, then there would have been no merges into master during the time that our feature branch existed.

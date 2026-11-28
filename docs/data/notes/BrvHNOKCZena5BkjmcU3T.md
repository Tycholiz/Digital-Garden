
## Centralized vs Distributed Git
We can run Git in a centralized system, where we have a git repo on a central server. All contributors must push and pull to the same repo for progress on the project to be made.
- A concern with this is that for anyone to be able to contribute, they must have access to the entire project. Therefore you cannot maintain control with a centralized workflow.
	- This may not be a concern, if there are 2 or 3 people working on a project.

With an integrator workflow, everyone has their own private repo and public repo. When a contributor wants to add their changes, they push to their own public repo. From there, we can pull their changes into our local repo to ensure everything works as expected. If everything is good, we merge them into our local branch, then push that branch to the main repo. From there, everyone can pull those changes.
	- with this workflow, everyone pulls from a single official repo, but pushes to their own public repo.

Centralized:
![](/assets/images/2021-03-11-15-39-35.png)
Distributed:
![](/assets/images/2021-03-11-15-39-48.png)

# Git branching strategies

Anuvaad follows the standard **feature-master** type of branching strategy for code maintenance. The releases happen through the master branch via release tags.

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Branches

### Feature Branches

Feature branches are a set of branches owned by individual developers in order to work on specific tasks. These branches are forked out of the master branch and they eventually feed into the same master branch once the code for that particular use case is developed and tested. These branches can either be deleted right after merging to master or can be retained to be reused for other use cases.

Feature branches can ONLY be deployed in the **‘Dev’ environment**. The ‘Dev’ environment is a dedicated VM for the developers to test their code. Once the code is dev-tested, it must be merged to the **‘develop’ branch** which further feeds into the **‘master’ branch**.

### Develop Branch

The ‘Develop’ branch is a mirror branch to the master branch. This branch is dedicated for QA/UAT testing. All feature branches must feed into this branch before the use-case is sent for QA testing and at times UAT if needed. This branch will also act as a backup in case there’s something wrong with the master branch.

The develop branch can ONLY be deployed to the **‘QA’ environment**. This is a dedicated environment for the QAs to perform unit, regression, and smoke testing of the features and the app as a whole. This environment can also be used for UAT purposes. Once there’s a QA signoff on the features, this will directly feed into master.

### Master Branch

The master branch is the main branch from which all releases happen. All features, once dev-tested and QA-tested, will feed into master via the develop branch. The master branch is from where the code is deployed to production. Every release to production from the master branch will be tagged with the specific version of that release.

In case of production issues, we can fallback to any of the previous stable releases.

### Hotfix Branches

Hotfix branches are temporary branches which are forked directly from the **‘master’ branch** and will feed back into the master only. These are for special cases when there’s a production bug to be resolved, and the develop branch is at the (n+m)th commit and master at (n)th commit.

These branches will act as temporary mirror branches for the master branch and can be tested on the **QA env**. Once tested and merged back to master, these branches have to be deleted. After the merge, the develop branch will have to be rebased with master, and the features will have to be rebased with the develop branch. The commits will flow upstream only after a rebase is successfully completed on all the forks.

Apart from the feature branches, individual devs will also own these branches.

## Code Check-in

* **Feature Branches:** Code check-in to feature branches can be done by anyone; there’s no need for a review as such. These branches are mainly for the devs to test their code. The use case developed in this branch will have to be dev-tested on the **‘Dev’ environment** before a merge request to the **‘develop’ branch** is raised.
* **Develop Branch:** Code check-in to the develop branch should only happen after a **Peer Review**. Merge to develop will only happen once the code is dev-tested on the Dev environment. It should be noted that a merge to develop should ensure that the code quality is up to the mark, all standards are followed, and it doesn’t break anything that is already merged to the develop branch by other devs. QA testing must happen on this branch deployed in a dedicated environment for QA/UAT. Any bugs reported will be fixed in the feature branch, reviewed, and then merged back to the develop branch. QA signoff happens on this branch.
* **Master Branch:** Code check-in to the master branch will only happen from the develop branch and NO feature branches. Any merge to the master branch apart from the hotfix branch MUST come from the develop branch only. Merge from develop to master should happen only after an extensive code review from the leads. Only a select few members of the team will have access to merge the code to the master branch. The onus of the master branch is on the Technology leads of the team. Once the code is merged to master, a final round of regression testing must take place before the code is tagged for release.
* **Hotfix Branch:** Code check-in to the hotfix branch can be done by individual devs once it is reviewed by a peer and the leads. This branch feeds into the master only after a second round of review. QA must happen on the hotfix branch before it is merged to master. The merge to master must also be released only after regression testing is done on the fix.

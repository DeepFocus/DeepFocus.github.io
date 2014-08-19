#Introduction

##Purpose of this Document

This document is indented to document an ideal process how to manage branches in the source code management tool GIT (Short: “GIT Branching Model”). This process may not apply (or be the best approach) to all projects. It is a work-in-progress document and should be updated and improved based on project experiences.

##Inspiration / Source    

The Deep Focus Branching Model is based on “A successful Git branching model”, published by Vincent Driessen (2010, http://nvie.com/posts/a-successful-git-branching-model/).

##Goals of the Model

The DF model will define naming conventions, branch roles and workflows fitting DF’s work environment and products. By those standards we want to improve the company’s efficiency. It will allow a faster on-boarding of new employees, be used for employee training. Next it will ease communication between all stakeholders of the software development process (engineers, QA, project management, account, client). Furthermore it will help to reduce regression bugs.
All those improvements will lead towards a better quality of work: It will prevent errors and bad deployments as well as assisting in having a better organized codebase. Altogether the resulting   code-tree will represent the team’s workflow.

#Main Branches   

As other branching models the DF model consists of two main branches in its core:

- master
- develop

###Master Branch
Master (on origin) is the main branch where the source code of HEAD always reflects a production-ready state. There are no commits happening in the master branch, it has the single use to document the release history, hence only merges happen within this branch.
###Develop Branch
Develop (on origin) is the main branch where the source code of HEAD always reflects a state with the latest delivered development changes for the next release. If continuous integration is used in a project, this branch should be the branch to deploy to the development test server.

Once develop reaches a “stable” state for the next release, the goal is to merge the code to master. To ensure a freedom from defects the release code will run thru a collection of supporting branches before eventually merging to master. This includes among others a qa branch and will be explained in the sections “xxx” & “Preparing a release”.

#Supporting Branches
In addition to our main branches the models uses supporting branches. The purpose of those branches is to aid parallel development between team members, ease tracking of features, prepare for production releases and to assist in quickly fixing live production problems.

There are long-living (qa and stage) and short-living (feature, release and hotfix) supporting branches.


The qa and stage branch are long-living branches as each of them reflects exactly one test system / server. All Short-Living branches are (usually) not deployed to test, qa, stage or production servers. In the moment a short-living branch is not needed anymore, it will eventually be deleted, while the long-living branches will live throughout the whole project lifecycle.

##Short-Living
The different types of branches we may use are:

- feature branches
- release branches
- hotfix branches
 
In contrast to the long-living branches there may be more than one instance of each branch type. The branches will be named by the type and a suffix, separated by a slash (“/”):

- feature/{feature-name}
- release/{release-version}
- hotfix/{hotfix-version} or hotfix/{hotfix-name}

##Long-Living
Besides the main long-living branches master & develop the model uses two supporting long-living branches to prepare production releases:

- qa
- stage


The qa branch is intended to be used for QA while the stage branch will be used for all client-facing, pre-production purposes.

#Supporting Branches in Detail
Previously we discussed all different branch types the model is using. In the following we’ll deepen the roles and tasks of each branch.    

##Feature Branch
- May branch off from: develop
- Must merge back into: develop
- Branch naming convention: feature/*

Feature branches (or sometimes called topic branches) are used to develop new features for the upcoming or a distant future release. When starting development of a feature, the target release in which this feature will be incorporated may well be unknown at that point. The essence of a feature branch is that it exists as long as the feature is in development, but will eventually be merged back into develop (to definitely add the new feature to the upcoming release) or discarded (in case of a disappointing experiment).

##Qa Branch
- May branch off from: develop
- Must merge back into: develop
- May merge into: stage
- Branch naming convention: qa
- New QA release tagged via: release-version/q-version 


Qa reflects a state with the latest QA-ready source code of the next release. While the QA team is testing a release the code shall not be changed. The detailed process for handling fixes of a qa branch needs further definition.

Depending on the project requirements this process may vary:

- Parallel progress on develop: Once QA fails back a release, fixes shall be done on the qa branch. All code changes need to be merged back to develop.
- “Feature freeze”: Fixes shall be done on the develop branch. Once all fixes are in a stable state, all changes are merged to qa 

###Alternative Proposal: Issue Branch
Depending on the project adding short-living “issue” branches could help the QA process. It will allow isolated bugfixing as well as keeping the qa branch free of actual commits. The issue branches need to be merged off of qa and be merged back to qa and develop.

###Tagging
Each new QA release should be tagged with the next release version and a QA release version. All (JIRA) tickets need to be tagged accordingly.

Examples:

- v1.0.0/q1.0.0
- v1.0.0/q1.0.1
- v1.0.0/q1.0.2
- v1.0.0/q1.0.3

##Stage Branch
- May branch off from: qa
- Must merge back into: develop
- May merge into: release
- Branch naming convention: stage

At the moment QA is passed the code is merged from qa to stage. The stage branch will be deployed to a (client-facing) test server. If stage needs to be updated as of client feedback, all changes need to go thru the previously described QA workflow.
As soon as the client approved the next release, a release branch will be created.

##Release Branch
- May branch off from: stage
- Must merge into: master
- Must merge back into: develop
- Merge into develop evokes merge form develop into: qa and stage
- Branch naming convention: release/*


Release branches support preparation of a new production release. They allow for last-minute dotting of i’s and crossing t’s. Furthermore, they allow for minor bug fixes and preparing meta-data for a release (version number, build dates, etc.).
The key moment to branch off a new release branch from stage is when stage (almost) reflects the desired state of the new release. At this moment the branches develop, stage and qa must have the same code base.

All features that are targeted for the release-to-be-built must be merged into develop, qa and stage at this point in time. All features targeted at future releases may not—they must wait until after the release branch is branched off.

Once the release is finished and the branch was merged back to develop, the code changes must be merged from develop to qa and stage.

##Stability
Another benefit of this model is to be able to identify the stability of code.

- Branch Name | Stability | QAed
- feature | low | no
- develop | medium | no / partly
- qa | high | partly / yes
- stage | very high | yes
- master | highest | yes
- hotfix | highest | yes / partly

##Flow Chart
Technology Department > Branching Model > git flow chart.jpg


{% include speakerdeck.html deckId="7b8b7b300a0201326f7c2216803d3d2d" %}
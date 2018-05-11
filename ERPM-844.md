# ERPM-844 Modify Branching Strategy for Catalog Integrations Team

> Guidance from management has suggested we move towards the GitHub Flow for our software development process. Let's keep in mind that the ultimate goals are to support regular (or continuous) deployments, and that the master branch is a representation of what is in production.

## Current Branching Workflow

- Create `feature` branch from `master`
- Develop locally, commmit often
- Create Pull Request from `feature` branch into `master` _(when development is complete)_
- Code Review, and merge into `master`
- __Build__ `master` branch
- __Deploy__ artifacts to DEV environment
- Verify new feature works in DEV environment
- __Deploy__ DEV artifacts to QA environment
- Verify new feature works in QA environment
- __Deploy__ QA artifacts to PROD environment
- Verify new feature works in PROD environment

## Current Environments

- DEV (Integration)
- QA
- PROD

## Current Issues

- We have no branch that simply represents what is in the production environment
	- We have to go back to the build server to see what commit hash we last deployed
	- The branch itself does not represent production, only a specific commit on that branch
- If working on one component, but multiple features in parallel, cannot simply deploy the first feature that is ready without deploying the other features, since all features currently in DEV/QA are on the master branch.

## GitHub Flow

> https://guides.github.com/introduction/flow/index.html

- Create `feature` branch from `master`
- Develop locally, commit often
- Create Pull Request from `feature` branch into `master` _(as early as possible)_
- Code Review
- __Build__ `feature` branch
- __Deploy__ artifacts to QA environment
- Verify new feature works in QA environment
- __Deploy__ artifacts to PROD environment
- Verify new feature works in PROD environment
- Merge `feature` branch into `master`

## Catalog Integrations Flow

- Create/Rebase `develop` branch from `master`
- Create `feature` branch from `develop`
- Develop locally, commit often
- Create Pull Request from `feature` branch into `develop` _(as early as possible)_
- Code Review, and merge into `develop`
- __Build__ `develop` branch
- __Deploy__ artifacts to DEV environment
- Verify new feature works in DEV environment
- __Deploy__ artifacts to QA environment
- Verify new feature works in QA environment
	- _Note: REBASE CONSTANTLY_

### Production Deployment options:

1. Deploy then Merge
	- Rebase `master` with `feature`
		- _Note: This should ensure that the feature branch has all previous features/commits._
		- If `feature` did not have all previous commits, go through the DEV/QA again
	- __Deploy__ artifacts to PROD environment
	- Verify new feature works in PROD environment
	- Create Pull Request from `feature` branch into `master`
		- Possibly tag the commit/merge
1. Merge then Deploy
	- Create Pull Request from `feature` branch into `master`
		- _Note: Do not merge develop into master because the develop branch contains code that's not ready. Only merge the feature-related commits into master._
	- __Build__ `master` branch
	- __Deploy__ artifacts to STAGE environment
	- Smoke test STAGE environment
	- __Deploy__ artifacts to PROD environment
	- Verify new feature works in PROD environment
	- Rebase `develop` branch from `master`


## Circle CI

We want to __BUILD__ once, and __DEPLOY__ as often as necessary.

## Roadmap

- [ ] Modify workflow steps in `.circleci/config.yml` file.
- [ ] Migrate projects to .NET Core.
- [ ] Migrate projects to `catalog-integrations` GitHub repository.
- [ ] Setup Circle CI builds against branches and pull requests.
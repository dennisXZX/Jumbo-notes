## Jumbo notes

#### Codebase

__Server entry point__

- web/server/index.js

- web/src/server/universial/main.js


__Routes__

- web/src/app/routes.js


__Statistics Page Task__

- UI Component

StatisticsCards.js

StatisticsTip.js

statisticsTip.css


- Storybook UI

StatisticsTip.story.js


- Data Provider

test/dataProviders/statistics/tips.js/hotNumbers


__How to start development environment__

1. `gijoe start cluster --lite` to launch all the containers for the dev environment

2. `gijoe list` to check if all the containers are up and running

You should see the following containers

- ozlotteries
- vault
- web
- jl
- db.jl
- audit-log
- attenborough
- whalens

3. If something goes wrong or you don't see some server is running. Shut them down and restart them.

`gijoe stop vault` to stop vault container

`gijoe start vault` to start vault container


__How Gerrit workflow works__

1. Create a story on JIRA

2. Create sub-tasks for your story

3. Create a branch on Gerrit

4. Fetch the branch onto your local machine and check out to the branch

5. If you haven't finished a sub-task but want to commit to Gerrit as a patch, the first time you should use `git commit`, then name your commit message using the format of `JIRA_ID SomeFeatureDescription`, then use `git push origin HEAD:refs/drafts/WEB-1990_SomeFeature`, which would not trigger CI. (IMPORTANT: any subsequent commits to this patch should use `git commit --amend` because this would update the existing patch instead of creating a new one.

6. If you finish a sub-task, you can push it to Gerrit using `git push origin HEAD:refs/for/WEB-1990_SomeFeature`, which would trigger a CI run

7. When all your sub-tasks related to the story are complete, you can merge your branch into `the_frontier`, which is a release candidate branch

8. `the_frontier` branch will finally be merged into `master`

__Questions in codebase__

BaseGameOffer.js – how react-motion works

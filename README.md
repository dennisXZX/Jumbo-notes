## Jumbo notes

#### Codebase

__Server entry point__

- web/server/index.js

- web/src/server/universial/main.js


__Routes__

- web/src/app/routes.js

__States__

- web/app/redux

__Storybook doesn't refresh after changes__

Use `sudo yarn storybook` to launch storybook.

__Run Flow on your project__

1. `gijoe enter web` to get into your web container

2. Use `./node_modules/.bin/flow` command to run Flow on your project

__Flow doesn't pick up your latest code__

1. Check the running process of flow by `ps aux | grep flow`

2. Kill the Flow processes using `sudo pkill -9 flow` and `sudo pkill -9 flow-bin`

__Build language files__

Anytime you add new language using `react-intl`, you need to run `yarn build:langs` to generate new translation JSON file.

__Upload static images__

For now, static images need to be uploaded to `JL` project.

1. Get into `JL` docker by using `gijoe enter jl`

2. Checkout `origin/the_frontier` branch

3. Open `JL` project in your editor

4. Place your files into `jl/web/media/images/pub/yourComponent`

5. Commit your patch by running `git commit`

The commit message should look as follows to meet the linter requirement:

```
yourBranchName descriptionOfYourPatch

as above
```

6. Push your change to Gerrit by `git push origin HEAD:refs/for/the_frontier`

__Spin up your development environment__

1. `gijoe start cluster --lite` to launch all the containers for the dev environment

2. `gijoe list` to check if all the containers are up and running

You should see the following containers

- ozlotteries
- vault (credentials generator)
- web (container for web project)
- jl
- db.jl
- audit-log
- attenborough
- whalens (dynamic DNS system)

3. If something goes wrong or you don't see some server is running. Shut them down and restart them.

`gijoe stop vault` to stop vault container

`gijoe start vault` to start vault container

NOTE: if you restart `vault` you also need to restart `nginx` by `gijoe start nginx AU`.

4. `gijoe enter web` to get into web container

5. Run `yarn start:dev` to spin up your development environment

__How Gerrit workflow works__

1. Create a story on JIRA

2. Create sub-tasks for your story

3. Create a branch on Gerrit

4. Fetch the branch onto your local machine and check out to the branch

5. If you haven't finished a sub-task but want to commit to Gerrit as a patch, the first time you should use `git commit`, then name your commit message using the format of `JIRA_ID SomeFeatureDescription`, then use `git push origin HEAD:refs/drafts/WEB-1990_branchName`, which would not trigger CI. (IMPORTANT: any subsequent commits to this patch should use `git commit --amend` because this would update the existing patch instead of creating a new one.

6. If you finish a sub-task, you can push it to Gerrit using `git push origin HEAD:refs/for/WEB-1990_branchName`, which would trigger a CI run

7. When all your sub-tasks related to the story are complete, you can merge your branch into `the_frontier`, which is a release candidate branch

8. `the_frontier` branch will finally be merged into `master`

__Questions in codebase__

BaseGameOffer.js – how react-motion works

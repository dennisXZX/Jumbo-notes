## Jumbo notes

__Compile SCSS to CSS__

Everytime you make change to a SCSS file, you would need to compile it into CSS one.

1. `npm install -g node-sass`

2. cd `gijoe/codebases/web/scss` 

3. run `yarn install`

4. cd `gijoe/codebases/web`

5. `node-sass scss/main.scss --output public/css --output-style compressed`

__Start the new dev environment__

1. `gijoe start webd`

2. `docker ps` to see a list of running containers

2. `docker logs web_webd_1 -f` to see all the logs of a particular container

__Permissions problem on config/logs__

1. Connect to the Node using `ssh jumbo@ci-node-016 -i ~/gijoe/codebases/ansible-jumbo/files/jumbo/ssh/id_rsa`. Change the node number in the command. Also, change the permission for id_rsa to `400` if it complains about permission is too open.

1. First check that `config` directory has root:root as the permissions.

```
sudo ls -l /home/jenkins/swarm/workspace/web.pipeline/web
```

If it does:

```
sudo rm -rf /home/jenkins/swarm/workspace/web.pipeline/web
```

Basically we are removing the web folder all together. Friendly reminder: `rm -rf` command is irreversible, so just be a little bit careful here. =P

__Use staging testing data__

In the web project, modify `.env` file to point discovery base URL to staging `DISCOVERY_BASE_URL=https://attenborough.frontier2.staging.ozl.jumdev.com`

__Merge master branch into feature branch__

1. `git fetch` to update all the branches

2. `git checkout featureBranch` to checkout feature branch

3. `git merge origin/master --no-ff` to merge master branch into feature branch, `--no-ff` ensures a merge message is always presented and no fast-forward would happen

4. `git commit` and write the message `Merge origin/master branch into featureBranch` 

5. `git push origin HEAD:refs/for/featureBranch` to push the feature branch onto Gerrit

6. Once the merged patch completed CI build, ask people with permission to merge the patch into the feature branch

7. Once the patch is successfully merged into the branch, click `rebase` in your patches to get the latest update

__Get back to a patch that you were previously working on__

1. `git reflog` to see all the patches

2. Checkout the patch that you want

__Update the front end and backend to the latest__

1. Checkout the latest `WEB` patch

2. Checkout the latest `ozl` branch, and run `make` if there is any database, route or API changes

3. Now your dev environment should be the latest both in the front and back ends

__80 port has already been used__

Run `pkill node` then restart the dev server by `yarn start:dev`

__Add new route to Nginx__

1. Enter `JL` by `gijoe enter jl`

2. Add a new route in `src/resources/conf/nginx_v2/locations/server_locations_au_www.conf`

__Error: EACCES: permission denied__

1. If you run into permission denied issue while `yarn build:langs`

2. You can run `sudo chown -R $USER:$GROUP ./node_modules/` to solve the permission issue

__Some endpoints doesn't work___

1. Enter `JL` by `gijoe enter jl`

2. Run `make` to update database

3. Run `yarn build:config` to update the config file

4. Restart nginx by `gijoe start nginx AU`

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

2. Checkout `origin/master` branch

3. Open `JL` project in your editor

4. Place your image files into `jl/web/media/images/pub/yourComponent`

5. Commit your patch by running `git commit`

The commit message should look as follows to meet the linter requirement:

```
yourBranchName descriptionOfYourPatch

as above
```

6. Push your change to Gerrit by `git push origin HEAD:refs/for/master`

__Spin up your development environment__

1. `gijoe start cluster --lite`(old way) or `gijoe start webd` (new way) to launch all the containers for the dev environment.

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

NOTE: if you restart `vault` you also need to also restart `nginx` by `gijoe start nginx AU`.

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

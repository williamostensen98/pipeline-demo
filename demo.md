## Find the repo on github and fork it
Fork the repository (upper right corner),
and then clone your fork into your machine `git clone {repo name}` which you can find under the `Code` button.

## Study the example in deploy.yml
What are we looking at? Try to understand what the different
steps does. Do not spend too much time on the commented out
docker stuff as this is only there to show you how it would look if we actually pushed to docker. We are only interested in integrating with the Slack API today.

### Task 1
When you think you understand the pipeline,
try to edit the `editme.txt` with some text, add your change
`git add .` and then commit with `git commit -m"First edit"`
followed by a `git push`. Now go back to your forked repository and look at the `Actions` tab. You will
see that the workflow called "Deploy" runs the test.

### Task 2
Now, try to push something that makes the test fail,
or edit the test directly to make it fail with the current `editme.txt`. How does that look?

## Task 3
It is common to not push directly to master/main, but rather
use something called a pull request. If you now study
the `PR.yml` file, you can see that it is configured to run
only for so called `Pull Requests`. Also, note that
the pipeline is a bit more complicated. There is
also two `jobs`, one called `test` and one called `report`.
The goal of `report` is to post to a slack channel that a PR is ready, but only if the test ran succesfully for the PR. This is signaled by this little block, which creates a dependency on a succesful run of the `test`job.
```yaml
    needs:
    - test
```
### Setup slack secret
To be able to push messages to a slack channel, you need a magic token. Look at the powerpoint presentation and copy it from there into Settings -> Secrets -> New Repository Secret into a variable called `SLACK_WEBHOOK_URL`.
Go into slack and join the channel: `#pipeline-demo-summer-2022`

## Try to create a Pull request
Create a branch of the repository by doing
`git checkout -b feature/new-feature`
This makes you jump into a git branch on your machine.
Now, edit the `editme.txt` in a way that does not break the test and **EDIT THE MESSAGE ARGUMENT OF THE SLACK JOB SO YOU CAN DISTINGUISH YOUR MESSAGE FROM THE OTHER**, and do
`git add .` followed by `git commit -m "new edit"` and then
 `git push --set-upstream origin feature/new-feature` to publish your branch. Git will give back a link that you can follow to get into the pull request view. Otherwise, go to your repository and it will suggest that you can now do a pull request.

After creating the PR, check the actions! How does it look now? Can you see that the view shows the dependency of `report` on `test`? Also, check the slack channel 
`#pipeline-demo-summer-2022`. If the test passed, your pipeline should now have sent a message in the channel with the content you typed in!

Now, merge the pull request into master if you wish, or try to edit the `pr.yaml` pipeline. If you feel finished, try to ask other groups if they need help or experiment with the pipeline by creating new jobs that depend on each other. 

## Advanced
If you are really curious and know a lot about Git from before, you could try to edit the deploy job to trigger when a so called `git tag` is done for a commit. A git tag creates an alias for a commit, which can then be pushed by doing `git push --tags` on master. This is a more common way to deploy after a succesful pull request, instead of always just pushing to master. Hint:
```
on:
  push:
    tags:
      - v1.0.0
```

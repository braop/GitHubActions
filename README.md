# GitHub Actions
Thi is a tool within GitHub that allows you to automate software development workflows. It enables you to create custom workflows triggered by events such as pushing code to a repository, opening a pull request, or creating an issue.

These workflows consist of a series of actions, such as building and testing code, deploying to a server, or publishing a package to a package manager.

GitHub Actions provides a convenient way to automate common development tasks, streamline your CI/CD pipeline, and improve collaboration among team members.


## Using GitHub Actions involves the following steps:

1. Create a workflow: Workflows are defined in YAML files that are stored in the .github/workflows directory of your GitHub repository. To create a workflow, you need to create a new YAML file in this directory and specify the trigger events and actions that should be taken.

2. Define trigger events: You can define when your workflow should be triggered by specifying events in the YAML file. For example, you can trigger the workflow whenever a code change is pushed to the repository, when a pull request is opened, or when an issue is created.

3. Define actions: Actions are the individual tasks that make up your workflow. Each action is defined in a block of YAML code and can include a variety of tasks, such as building and testing code, deploying code to a server, or publishing a package to a package manager. Actions can be written in JavaScript or any other language that can be run on Linux, Windows, or macOS.

4. Run the workflow: After you've defined your trigger events and actions, you can run your workflow by committing the changes to your GitHub repository. GitHub Actions will automatically run the workflow whenever the specified trigger events occur.

5. Monitor the workflow: You can monitor the progress of your workflow from the Actions tab of your GitHub repository. You can view the status of each step in the workflow, including whether it succeeded or failed, and you can view detailed logs for each action.


##  Example of YAML file.

```
name: Github Action Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v3
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
   ```
   
### Definitions
- github.actor: retuns username of github account
- github.event_name: 
- runner.os: 
- github.ref:
- github.repository: 
- github.workspace: 
- job.status: 


## GitHub Actions key components
### Workflows:
These are the main component of GitHub Actions and are defined in YAML files stored in the .github/workflows directory of your GitHub repository. Workflows specify the trigger events and actions that should be taken in response to those events.

### Actions:
These are the individual tasks that make up a workflow. Each action is defined in a block of YAML code and can include a variety of tasks, such as building and testing code, deploying code to a server, or publishing a package to a package manager.

### Trigger events:
Trigger events are the events that cause a workflow to run, such as pushing code to a repository, opening a pull request, or creating an issue. You can specify the trigger events in the YAML file for your workflow.

### Runner:
This is the component that actually executes the actions in a workflow. GitHub Actions supports runners for Linux, Windows, and macOS, so you can write actions in a variety of programming languages and run them on the appropriate platform.

###  Environment variables:
These are used to store information that can be used by actions within a workflow. You can use environment variables to pass information between actions, store configuration settings, or set up secrets.

### Artifacts: 
These are files or data that are generated by a workflow and can be used by other workflows or jobs. Artifacts are stored on the GitHub servers and can be downloaded for use in other parts of your development process. 

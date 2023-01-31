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
- ${{ github.actor }}: 
Retuns username of github account

- ${{ github.event_name }}:
Returns the event name eg push, pull request etc

- ${{ runner.os }}:
Returns operating system on which the server is running

- ${{ github.ref }}:
Returns reference of branch eg refs/heads/master

- ${{ github.repository }}:
Returns github repository name

- ls ${{ github.workspace }}:
Lists the files in the workspace

- ${{ job.status }}:
Show status of the job eg success ot failed


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

## Using Github Actions to publish to Google Play Store
To use GitHub Actions to publish an app to the Google Play Store, you need to create a workflow that performs the following steps:

#### Step One
1. Build the app:
The first step is to build the app and make sure it is ready for release. You can use a GitHub Actions workflow to build the app and run any necessary tests to ensure it is working correctly.

```
name: Build and Test App

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up JDK
      uses: actions/setup-java@v1
      with:
        java-version: 11

    - name: Build the app
      run: ./gradlew build

    - name: Test the app
      run: ./gradlew test

```
#### Explanation of the workflow
The above workflow will run whenever a change is pushed to the master branch of the repository and it will perform the following steps:
1. Check out the code: The first step is to check out the code from your repository using the actions/checkout action.
2. Set up JDK: The next step is to set up the Java Development Kit (JDK) using the actions/setup-java action. In this example, the JDK version is set to 11. 
3. Build the app: The next step is to build the app using the Gradle build system. You can run the gradlew build command to build the app. 
4. Test the app: Finally, the app is tested using the Gradle test command. This step will run any tests that are included in your app and ensure that everything is working correctly.
      
#### Step Two
2. Sign the app: 
Before you can publish an app to the Google Play Store, you need to sign it with a unique key. You can use the GitHub Actions workflow to sign the app using the Android Signing Plugin.

#### Step Three
3. Upload the app: 
Once the app is signed, you can use the GitHub Actions workflow to upload it to the Google Play Store. You can use the Google Play Developer API to upload the app and publish it to the store.

#### Step Four
4. Build the app:
Monitor the release: After the app is published, you can use GitHub Actions to monitor the release and ensure that it was successful. You can use the Google Play Developer API to check the status of the release and receive notifications when it is complete.


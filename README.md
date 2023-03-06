## How to use Github Actions to publish AAB to Google Play Store.

To use GitHub Actions to publish an app to the Google Play Store, you need to create a workflow that
performs the following steps:

#### Step One

1. Build the app:
   The first step is to build the app and make sure it is ready for release. You can use a GitHub
   Actions workflow to build the app and run any necessary tests to ensure it is working correctly.


#### Step Two

2. Sign the app:
   Before you can publish an app to the Google Play Store, you need to sign it with a unique key.
   You can use the GitHub Actions workflow to sign the app using the Android Signing Plugin.


#### Step Three

3. Upload the app:
   Once the app is signed, you can use the GitHub Actions workflow to upload it to the Google Play
   Store. You can use the Google Play Developer API to upload the app and publish it to the store.

-----

In this sample project we use two workflows: 
1. build.yml: Builds and tests the app whenever commits are pushed to any branch except master branch.
2. deploy.yml: Signs the app and uploads it to google play whenever commits are pushed or merged to master branch.

### Workflow files Directory
In the root folder of your project, Create a folder and name it ".github" and in it create a folder and name it "workflows". Add your .yml files (build.yml and deploy.ml) in the workflows folder.

### Note
Before automating this process, The App must be in production on google play store, meaning the first publish to google play store is done manually via the google play console.

-----

### Requirements
1. Key.jsk: Created while signing app via android studio
2. Key password: Created while signing app via android studio
3. Key store password: Created while signing app via android studio
4. Alias: Created while signing app via android studio
5. Service Account Json: Contains the credentials for the Google Play Developer API

#### Github Repo Setup :: Create Secrets

In GitHub, you can create a secrets in your repository by following these steps:

1. Go to the repository you want to create a secret in.
2. Click on the "Settings" tab.
3. In the left navigation panel, click on "Secrets and Variables."
4. Click on Actions option.
5. Click on the "New repository secret" button.
6. Enter a name for the secret and its value.
7. Click on the "Add secret" button.


### Create Secrets
Go to github repository -> Settings -> Secrets and Variables (Left menu) -> Actions -> New Secret Repository and create the following:
1. ALIAS: (same as one used while generating the jsk)
2. KEY_PASSWORD: (same as one used while generating the jsk)
3. KEY_STORE_PASSWORD: (same as one used while generating the jsk)
4. SIGNING_KEY: To create this convert the .jsk key (generated from android) to base64(Use this link(https://8gwifi.org/Base64Functions.jsp)) and paste the string in secrets value.
5. SERVICE_ACCOUNT_JSON: Open json and copy and paste its in secrets value.

![creating_secrets](https://user-images.githubusercontent.com/25560375/219327104-b202f012-ec3d-4da5-83ac-6059655c3f30.png)

-----

#### Creating service account.

1. Go to google cloud console (https://console.cloud.google.com).
2. Using google menu navigate to IAM and Admin

![menu_google_cloud](https://user-images.githubusercontent.com/25560375/219331800-2346575b-ad83-4be9-83fd-0be937e09b02.png)

4. Select project or create project

![create_project](https://user-images.githubusercontent.com/25560375/219334773-a07a529f-3f48-4755-a188-7672551b447c.png)

6. Naviagte to service Account

![service_account](https://user-images.githubusercontent.com/25560375/219335313-0ac811c0-c136-45bb-b81c-d6b67fc9cc39.png)

6. Create searvice account
7. Enter service details
8. After entering service account name coppy email address undeneath service account ID

![service](https://user-images.githubusercontent.com/25560375/219336404-c98d3692-6897-488e-91fe-d0e4640effdd.png)

10. You may enter service account description(optional)
11. Click create and continue
12. Dont skip role even though it optional
13. Select: Role as Owner.

![role_owner](https://user-images.githubusercontent.com/25560375/219337058-71aad30d-ff4f-4d91-bb35-136388592908.png)

15. Press continue
16. Grant user accesss
17. Paste email to service account user role and service account admins role and click done.

![paste_email](https://user-images.githubusercontent.com/25560375/219339688-ce2cbb95-e054-49d9-a490-e502edd03d8f.png)

19. Manage key and add key to create key select json and a it will be downloaded.

![manage_key](https://user-images.githubusercontent.com/25560375/219338742-0b065921-20d7-47aa-b340-5115dc4b9ea3.png)

20. Add key -> create new key -> slecte json and service json file will be downloaded.

![create_project](https://user-images.githubusercontent.com/25560375/219341742-1b833939-d02d-4fda-a8c7-5c9cd5024203.png)

21. Go back to google cloud menu and click on Api servies, enable api

![api_enable](https://user-images.githubusercontent.com/25560375/219340842-684ec46b-f336-4f6b-8d12-aea78f2fb0e0.png)

23. Search for Google Play Android Developer API and enable it.

![search_api](https://user-images.githubusercontent.com/25560375/219342325-70865497-2eb3-4228-818c-3bb5ec37a5ca.png)
![search_results](https://user-images.githubusercontent.com/25560375/219342875-47fc57b3-b058-4891-b53d-c0b0bcb156dc.png)

24. Enable api

![enable_api](https://user-images.githubusercontent.com/25560375/219343316-719e7d1c-1fe1-43c2-9740-92047806ea14.png)

-----

25. Got to google play console - > Setup - Api acess

- Link existing google cloud project
- Select from drop down and save

![link_api](https://user-images.githubusercontent.com/25560375/219344333-711c1efb-2a19-4068-bc4c-aa636913fbcf.png)
![project_list](https://user-images.githubusercontent.com/25560375/219344823-ebf6e442-a720-470f-ab81-e28142baaabe.png)

26. Should appear under service account.

![under_service_account](https://user-images.githubusercontent.com/25560375/219345570-699f64be-9b38-44e2-a423-00d82f02d6a6.png)

27. Click on manager service account and in Account permission add Admin(all permissions) and save changes

28. Under App permissions add apps that u want to be managed.

29. GO to users and permissions -> manage users and add service account id/ email (from google cloud after creating service account name).


### build.yml
For building and testing whenever any commit is pushed to any branch rather than master branch.
```
name: Continous Integration

on:
  push:
    branches:
      - '*'
      - '!master'

jobs:
  build:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout
        uses: actions/checkout@v3

      - name: set up JDK
        uses: actions/setup-java@v3
        with:
          java-version: 11
          distribution: 'temurin'

      - name: Grant rights
        run: chmod +x gradlew

      - name: Build with Gradle
        id: build
        run: ./gradlew build

      - name: Test the app
        run: ./gradlew test


```


### deploy.yml 
For plushing app to google play store whenever commits are pushed to master branch
```
name: Continous Deployment

on:
  push:
    branches: [ master ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout
        uses: actions/checkout@v3

      - name: set up JDK
        uses: actions/setup-java@v3
        with:
          java-version: 11
          distribution: 'temurin'

      - name: Grant rights
        run: chmod +x gradlew

      - name: Build with Gradle
        id: build
        run: ./gradlew build

      - name: Build Release AAB
        id: buildRelease
        run: ./gradlew bundleRelease

      - name: Sign AAB
        id: sign
        uses: r0adkll/sign-android-release@v1
        with:
          releaseDirectory: app/build/outputs/bundle/release
          signingKeyBase64: ${{ secrets.SIGNING_KEY }}
          alias: ${{ secrets.ALIAS }}
          keyStorePassword: ${{ secrets.KEY_STORE_PASSWORD }}
          keyPassword: ${{ secrets.KEY_PASSWORD }}

      - name: Create service_account.json
        id: createServiceAccount
        run: echo '${{ secrets.SERVICE_ACCOUNT_JSON }}' > service_account.json

      - name: Deploy to Play Store
        id: deploy
        uses: r0adkll/upload-google-play@v1
        with:
          serviceAccountJson: service_account.json
          packageName: com.example.GitHubActions
          releaseFiles: app/build/outputs/bundle/release/app-release.aab
          track: production
```

### Workflow Monitoring
Go to repository and click on actions on the menu.

![monitor_workflow](https://user-images.githubusercontent.com/25560375/219348214-f12ca81f-51e3-4701-974f-cfdb5ea2c4b7.png)

### Pay attention to following to avoid errors
1. Granting rights. 
2. JDK version to use. 
3. Track: Make sure the track your publishing your app to is avaialable on google play console.
4. Lint check: if the lint check fails, the app fails to build. Ensure lint check is passed.

### Note
1. Always update the version code before merging to master so as to avoid conflicts with the already published ABB on google play.


## How to use Github Actions to Distribute development app via firebase.
1. distribute.yml: Builds and generates apk and distributes it to firebase app distribution.

```
name: Build & upload to Firebase App Distribution

on:
push:
branches: [ dev_firebase ]

jobs:
build:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout
        uses: actions/checkout@v3

      - name: set up JDK
        uses: actions/setup-java@v3
        with:
          java-version: 11
          distribution: 'temurin'

      - name: Grant rights
        run: chmod +x gradlew

      - name: build release
        run: ./gradlew assembleRelease
      - name: upload artifact to Firebase App Distribution
        uses: wzieba/Firebase-Distribution-Github-Action@v1
        with:
          appId: ${{secrets.FIREBASE_APP_ID}}
          token: ${{secrets.FIREBASE_TOKEN}}
          #serviceCredentialsFileContent: ${{ secrets.CREDENTIAL_FILE_CONTENT }}
          groups: testers
          file: app/build/outputs/apk/release/app-release-unsigned.apk
```

2. Add create project on firebase and app and install firebase sdk to android as instructed
3. Add secrets to github: FIREBASE_APP_ID and FIREBASE_TOKEN
4. Add tester to firebase app distribution

### Firebase Configuration
1. Go to the Firebase console and create a project. 
2. Complete the app’s firebase configuration. 
3. Create an android app in firebase and copy the App ID in the firebase project settings.
![app_id](https://user-images.githubusercontent.com/25560375/223026208-e9eb3670-8876-4ed7-a9d6-4f765e2b82e5.png)
4. Install and configure the firebase on your machine. 
5. Go to Release & Monitor > App Distribution > Accept the Terms of services and click Get Started. 
6. Go to Testers & Groups and add a new group called testers. 
![firebase_distribution](https://user-images.githubusercontent.com/25560375/223025676-a0abedae-80e5-4767-952a-23938a1a2040.png)
7. Click the New link under Invite links, choose the testing group, and then click Create link. A fresh public link will be created to invite other testers. 
8. We will implement wzieba/Firebase-Distribution-Github-Action@v1 and supply the following GH secrets to publish the APK to the Firebase distribution app. 
9. Generate the firebase token — https://firebase.google.com/docs/cli#cli-ci-systems and copy the token.
![firebase_token](https://user-images.githubusercontent.com/25560375/223025247-a35724ad-25d0-4897-a2b8-13f82481af08.png)



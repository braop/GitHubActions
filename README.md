## Using Github Actions to Publish ANDROID APP BUNDLE (AAB) to Google Play Store.

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

In this sample project we use two workflows: one that build the and test the app whenever commits are pushed to any branc rather than master and one that uploads app to google play whenever commits are pushed or merged to master branch.

### Note
Before automating this process, App must be in production on google play, meaning the first publish to google play is done manually via the google play console.

### Requirements
1. Key.jsk: Created while signing app via android studio
2. Key password: Created while signing app via android studio
3. Key store password: Created while signing app via android studio
4. Alias: Created while signing app via android studio
5. service account json: Contains the credentials for the Google Play Developer API

#### Github Repo Setup :: Create Secrets

In GitHub, you can create a secrets in your repository by following these steps:

1. Go to the repository you want to create a secret in.
2. Click on the "Settings" tab.
3. In the left navigation panel, click on "Secrets and Variables."
4. Click on Actions option.
5. Click on the "New repository secret" button.
6. Enter a name for the secret and its value.
7. Click on the "Add secret" button.

Note: Create secrets for:
1. KEY_STORE_PASSWOR => Paste key store password used while creating sign in key(key.jsk)
2. KEY_PASSWORD  => Paste key password used while creating sign in key(key.jsk)
3. ALIAS =>  Paste alias used while creating sign in key(key.jsk)
4. SIGNING_KEY => Paste base64 string: Use this link(https://8gwifi.org/Base64Functions.jsp) and convert file to base64
5. SERVICE_ACCOUNT_JSON => Paste content of service json file

![creating_secrets](https://user-images.githubusercontent.com/25560375/219327104-b202f012-ec3d-4da5-83ac-6059655c3f30.png)


#### Creating service account.

1. Go to google cloud console (https://console.cloud.google.com).
2. Using google menu navigate to IAM and Admin

![menu_google_cloud](https://user-images.githubusercontent.com/25560375/219331800-2346575b-ad83-4be9-83fd-0be937e09b02.png)

4. Naviagte to service Account


6. Create searvice account
7. Enter service details
8. After entering service account name coppy email address undeneath service account ID
9. You may enter service account description(optional)
10. Click create and countine
11. Dont skip role even though it optional
12. Select: Role as Owner.
13. press countinue
14. Grant user accesss
15. paste email to service account user role and service account admins role and cline done.
16. Manage key and add key to create key select json and a it will be downloaded.
17. Go back to google cloud menu and click on Api servies, enable api
18. Search for Google Play Android Developer API and enable it.


### On_Push_CI.yml
```name: Continous Integration

on:
  push:
    branches:
      - '*'
      - '!master'

jobs:
  build:

    runs-on: ubuntu-latest

    #    env:
    #      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}

    steps:

      - name: Checkout
        uses: actions/checkout@v2

      - name: set up JDK
        uses: actions/setup-java@v1
        with:
          java-version: 11

      - name: Grant rights
        run: chmod +x build.gradle

      - name: Build with Gradle
        id: build
        run: ./gradlew build

      - name: Test the app
        run: ./gradlew test

```

### Deploy_CI.yml
```
name: Continous Deployment

on:
push:
branches: [ dev ]

jobs:
build:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout
        uses: actions/checkout@v2

      - name: set up JDK
        uses: actions/setup-java@v1
        with:
          java-version: 11

      - name: Grant rights
        run: chmod +x build.gradle

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

      - name: Deploy to Play Store (BETA)
        id: deploy
        uses: r0adkll/upload-google-play@v1
        with:
          serviceAccountJson: service_account.json
          packageName: com.package
          releaseFiles: app/build/outputs/bundle/release/app-release.aab
          track: beta
#          whatsNewDirectory: whatsnew/

```

### Create Secrets
Go to github repository -> Settings -> Secrets and Variables (Left menu) -> Actions -> New Secret Repository and create the following:
1. ALIAS: (same as one used while generating the jsk)
2. KEY_PASSWORD: (same as one used while generating the jsk)
3. KEY_STORE_PASSWORD: (same as one used while generating the jsk)
4. SIGNING_KEY: To create this convert the .jsk key (generated from android) to base 64 and paste the string in secrets value.
5. SERVICE_ACCOUNT_JSON: Open json and copy and paste its in secrets value.

### How to create: SERVICE_ACCOUNT_JSON from google play store



set up SonarCloud
1. Create a SonarCloud Account:
Go to sonarcloud.io and sign up/log in (you can use your GitHub/GitLab/Bitbucket account).

2. Create a New Project:
Once logged in, click the "+" icon or "Analyze new project".
Import your organization from GitHub/GitLab/Bitbucket.
Select the repository where you pushed your index.html.
Choose "With Jenkins" for the analysis method.
Follow the instructions to set up the project. SonarCloud will give you a Project Key and a Token. Keep these safe.

Part 3: Set up Jenkins
You'll need a running Jenkins instance. If you don't have one, the easiest way is with Docker:

2. Install Necessary Jenkins Plugins:
Go to Manage Jenkins > Manage Plugins > Available plugins and install:
SonarQube Scanner
Pipeline (usually installed by default)
Git (usually installed by default)
3. Configure SonarQube Scanner Global Tool:
Go to Manage Jenkins > Global Tool Configuration.
Scroll to "SonarQube Scanner".
Click "Add SonarQube Scanner".
Give it a Name (e.g., SonarScanner).
Select "Install automatically" and choose the latest version.
Click Save.
4. Configure SonarCloud in Jenkins:
Go to Manage Jenkins > Configure System.
Scroll down to "SonarQube servers".
Click "Add SonarQube".
Name: SonarCloud (or similar)
Server URL: https://sonarcloud.io
Authentication Token:
Click "Add" > "Jenkins".
Kind: "Secret text"
Secret: Paste your SonarCloud user token (the one you generated on SonarCloud for your account).
ID: sonarcloud-token (or similar, remember this ID)
Description: SonarCloud User Token
Click Add.
Select the newly created token from the dropdown.
Click Save.



Part 4: Create Jenkins Pipeline
Now, let's define the CI/CD pipeline in a Jenkinsfile that Jenkins will use.
1. Create Jenkinsfile in your repository:
Place this file at the root of your Git repository, alongside index.html




Part 5: Create Jenkins Job
1. Create a New Item in Jenkins:
From the Jenkins dashboard, click "New Item".
Enter an Item Name (e.g., simple-html-cicd).
Select "Pipeline".
Click OK.
2. Configure the Pipeline Job:
General: (Optional) Add a description.
Build Triggers:
You can set up GitHub hook trigger for GITScm polling or Poll SCM if you want automatic builds on commit. For this guide, we'll manually trigger it.
Pipeline:
Definition: Select "Pipeline script from SCM".
SCM: Select "Git".
Repository URL: Paste the URL of your Git repository (e.g., https://github.com/<YOUR_GITHUB_USERNAME>/<YOUR_REPO_NAME>.git).
Credentials: If your repository is private, add your GitHub credentials here.
Branches to build: */main (or */master if that's your branch name).
Script Path: Jenkinsfile (this is the default, ensure your file is named exactly this).
Click Save.
Part 6: Run the Pipeline and View Results
1. Trigger the Build:
Go to your new Jenkins job (simple-html-cicd).
Click "Build Now" in the left-hand menu.
2. Monitor the Build:
Watch the build history. Click on the running build number and then "Console Output" to see the logs.
The SonarQube Analysis stage will run, and you should see output indicating that the scanner is sending data to SonarCloud.
The Quality Gate Check stage will wait for SonarCloud to report back.
3. View Results on SonarCloud:
After the Jenkins build completes successfully, go back to your SonarCloud project page.
You should see the analysis results for your index.html file. It should detect the minor "quality issue" we intentionally introduced (the x == "10" comparison).
The Quality Gate should pass (unless you have very strict default rules).

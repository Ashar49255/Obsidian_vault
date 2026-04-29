![[Screenshot from 2026-04-02 19-33-37.png]]

```# Objectives

In this lab, you will learn how to perform the following tasks:

- Create a Git repository on GitHub
- Create a simple Python application
- Test Your web application in Cloud Shell
- Define a Docker build
- Manage Docker images with Cloud Build and Artifact Registry
- Automate builds with triggers
- Test your build changes

## Prerequisites

If you do not already have a GitHub account, you will need to [create a GitHub account](https://github.com/signup)

**Recommendations**

1. Use an existing GitHub account if you have one. GitHub is more likely to block a new account as spam.
2. [Configure two-factor authentication on your GitHub account](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) to reduce the chances of your account being marked as spam.
```

```
## Task 1. Create a Git repository

First, you will create a Git repository using the GitHub. This Git repository will be used to store your source code. Eventually, you will create a build trigger that starts a continuous integration pipeline when code is pushed to it.

1. Click **Cloud Console**, and in the new tab click **Activate Cloud Shell** ().
2. If prompted, click **Continue**.
3. Run the following command to install the **GitHub CLI**:

curl -sS https://webi.sh/gh | sh

Copied!

4. Log in to the GitHub CLI

gh auth login

Copied!

Press `Enter` to accept the default options. Read the instructions in the CLI tool to log in through the GitHub website.

5. Confirm you are logged in:

gh api user -q ".login"

Copied!

If you have logged in successfully, this should output your GitHub username.

6. Create a `GITHUB_USERNAME` variable

GITHUB_USERNAME=$(gh api user -q ".login")

Copied!

7. Confirm you have created the environment variable:

echo ${GITHUB_USERNAME}

Copied!

If you have successfully created the variable, this should output your GitHub username.

8. Set your global git credentials:

git config --global user.name "${GITHUB_USERNAME}"
git config --global user.email "${USER_EMAIL}"

Copied!

This command creates a git user for your Cloud Shell terminal.

9. Create an empty GitHub repository named **devops-repo**:

gh repo create devops-repo --private

Copied!

10. Enter the following command in Cloud Shell to create a folder called `gcp-course`:

mkdir gcp-course

Copied!

11. Change to the folder you just created:

cd gcp-course

Copied!

12. Now clone the empty repository you just created. If prompted, click **Authorize**:

gh repo clone devops-repo

Copied!

**Note:** You may see a warning that you have cloned an empty repository. That is expected at this point.

13. The previous command created an empty folder called `devops-repo`. Change to that folder:

cd devops-repo

Copied!

## Task 2. Create a simple Python application

You need some source code to manage. So, you will create a simple Python Flask web application. The application will be only slightly better than "hello world", but it will be good enough to test the pipeline you will build.

1. In Cloud Shell, click **Open Editor** () to open the code editor.
2. Select the **gcp-course > devops-repo** folder in the explorer tree on the left.
3. Click on **devops-repo**.
4. Click **New File**.
5. Name the file `main.py` and press **Enter**.
6. Paste the following code into the file you just created:

from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def main():
    model = {"title": "Hello DevOps Fans."}
    return render_template('index.html', model=model)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=8080, debug=True, threaded=True)

Copied!

7. To save your changes. Press **CTRL + S**.
8. Click on the `devops-repo` folder.
9. Click **New Folder**.
10. Name the folder `templates` and press **Enter**.
11. Right-click on the `templates` folder and create a new file called `layout.html`.
12. Add the following code and save the file as you did before:

<!doctype html>
<html lang="en">
<head>
    <title>{{model.title}}</title>
    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">

</head>
<body>
    <div class="container">

        {% block content %}{% endblock %}

        <footer></footer>
    </div>
</body>
</html>

Copied!

13. Also in the templates folder, add another new file called `index.html`.
    
14. Add the following code and save the file as you did before:
    

{% extends "layout.html" %}
{% block content %}
<div class="jumbotron">
    <div class="container">
        <h1>{{model.title}}</h1>
    </div>
</div>
{% endblock %}

Copied!

15. In Python, application prerequisites are managed using pip. Now you will add a file that lists the requirements for this application.
    
16. In the **devops-repo** folder (_not the templates folder_), create a **New File** and add the following to that file and save it as `requirements.txt`:
    

Flask>=2.0.3

Copied!

17. You have some files now, so save them to the repository. First, you need to add all the files you created to your local Git repo. Click **Open Terminal** and in Cloud Shell, enter the following code:

cd ~/gcp-course/devops-repo
git add --all

Copied!

18. To commit changes to the repository, you have to identify yourself. Enter the following commands, but with your information (_you can just use your Gmail address or any other email address_):

git config --global user.email "you@example.com"
git config --global user.name "Your Name"

Copied!

19. Now, commit the changes locally:

git commit -a -m "Initial Commit"

Copied!

20. You committed the changes locally, but have not updated the Git repository you created in Cloud Source Repositories. Enter the following command to push your changes to the cloud:

git push origin main

Copied!

21. Refresh the **GitHub** web page. You should see the files you just created.

## Task 3. Define a Docker build

The first step to using Docker is to create a file called **Dockerfile**. This file defines how a Docker container is constructed. You will do that now.

1. Click **Open Editor**, and expand the **gcp-course/devops-repo** folder. With the **devops-repo** folder selected, click **New File** and name the new file **Dockerfile**.

The file _Dockerfile_ is used to define how the container is built.

2. At the top of the file, enter the following:

FROM python:3.13

Copied!

This is the base image. You could choose many base images. In this case, you are using one with Python already installed on it.

3. Enter the following:

WORKDIR /app
COPY . .

Copied!

These lines copy the source code from the current folder into the /app folder in the container image.

4. Enter the following:

RUN pip install gunicorn
RUN pip install -r requirements.txt

Copied!

This uses pip to install the requirements of the Python application into the container. Gunicorn is a Python web server that will be used to run the web app.

5. Enter the following:

ENV PORT=80
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 main:app

Copied!

The environment variable sets the port that the application will run on (in this case, 80). The last line runs the web app using the gunicorn web server.

6. Verify that the completed file looks as follows and save it:

FROM python:3.13
WORKDIR /app
COPY . .
RUN pip install gunicorn
RUN pip install -r requirements.txt
ENV PORT=80
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 main:app

Copied!

## Task 4. Manage Docker images with Cloud Build and Artifact Registry

The Docker image has to be built and then stored somewhere. You will use **Cloud Build** and **Artifact Registry**.

1. Click **Open Terminal** to return to Cloud Shell. Make sure you are in the right folder:

cd ~/gcp-course/devops-repo

Copied!

2. The Cloud Shell environment variable DEVSHELL_PROJECT_ID automatically has your current project ID stored. The project ID is required to store images in Artifact Registry. Enter the following command to view your project ID:

echo $DEVSHELL_PROJECT_ID

Copied!

3. Enter the following command to create an Artifact Registry repository named devops-repo:

gcloud artifacts repositories create devops-repo \
    --repository-format=docker \
    --location=us-east4

Copied!

4. To configure Docker to authenticate to the Artifact Registry Docker repository, enter the following command:

gcloud auth configure-docker us-east4-docker.pkg.dev

Copied!

5. To use Cloud Build to create the image and store it in Artifact Registry, type the following command:

gcloud builds submit --tag us-east4-docker.pkg.dev/$DEVSHELL_PROJECT_ID/devops-repo/devops-image:v0.1 .

Copied!

_Notice the environment variable in the command_. The image will be stored in Artifact Registry.

6. On the Google Cloud console title bar, type **Artifact Registry** in the **Search** field, then click **Artifact Registry** in the search results.
    
7. Click on the **Pin** icon next to **Artifact Registry**.
    
8. Click **devops-repo**.
    
9. Click **devops-image**. Your image should be listed.
    
10. On the Google Cloud console title bar, type **Cloud Build** in the **Search** field, then click **Cloud Build** in the search results.
    
11. Click on the **Pin** icon next to **Cloud Build**.
    
12. Your build should be listed in the history.
    

You will now try running this image from a Compute Engine virtual machine.

13. On the **Navigation menu**, click **Compute Engine > VM Instance**.
    
14. Click **Create Instance** to create a VM.
    
15. On the **Create an instance** page, specify the following, and leave the remaining settings as their defaults:
    

|**Property**|**Value**|
|---|---|
|**OS and storage > Container**|Click DEPLOY CONTAINER|
|**Container image**|'`us-east4`-docker.pkg.dev/`qwiklabs-gcp-00-3688114e90f5`/devops-repo/devops-image:v0.1` and click SELECT|
|**Networking > Firewall**|Allow HTTP traffic|

16. Click **Create**.
    
17. Once the VM starts, click the VM's external IP address. A browser tab opens and the page displays `Hello DevOps Fans.`
    

**Note:** You might have to wait a minute or so after the VM is created for the Docker container to start.

18. You will now save your changes to your Git repository. In Cloud Shell, enter the following to make sure you are in the right folder and add your new Dockerfile to Git:

cd ~/gcp-course/devops-repo
git add --all

Copied!

19. Commit your changes locally:

git commit -am "Added Docker Support"

Copied!

20. Push your changes to Cloud Source Repositories:

git push origin main

Copied!

Click **Check my progress** to verify the objective.

Please create a VM instance with given configuration

Manage Docker images with Cloud Build and Artifact Registry.

Please create a VM instance with given configuration

## Task 5. Automate builds with triggers

1. On the **Navigation menu**, click **Cloud Build**. The **Build history** page should open, and one or more builds should be in your history.
    
2. Click **Permissions**.
    
3. From Service account dropdown, select `qwiklabs-gcp-00-3688114e90f5`@`qwiklabs-gcp-00-3688114e90f5`.iam.gserviceaccount.com
    
4. Enable the **Set as pre-selected for new triggers** option. Set the status of the `Cloud Build` service to **Enabled**.
    
5. Go to **Triggers** in the left navigation and click **Create trigger** .
    
6. Specify the following:
    
    **Name**: devops-trigger
    
    **Region**: `us-east4`
    
    For **Repository**, click **Connect new repopository**
    
    - In the **Connect repository** pane select **GitHub (Cloud Build GitHub App)** and click Continue.
    - Select {your github username}/devops-repo as **Repopository**, click OK then select {your github username}/devops-repo (GitHub App).
    - Accept the terms and conditions and click **Connect**
    
    **Branch**: .*(any branch)
    
    **Configuration Type**: Cloud Build configuration file (yaml or json)
    
    **Location**: Inline
    
7. Click **Open Editor** and replace the code with the code mentioned below and click **Done**.
    

steps:
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build', '-t', 'us-east4-docker.pkg.dev/qwiklabs-gcp-00-3688114e90f5/devops-repo/devops-image:$COMMIT_SHA', '.']
images:
  - 'us-east4-docker.pkg.dev/qwiklabs-gcp-00-3688114e90f5/devops-repo/devops-image:$COMMIT_SHA'
options:
  logging: CLOUD_LOGGING_ONLY

Copied!

8. For **Service account** select the service account starting with your project-id that look similar to (`qwiklabs-gcp-00-3688114e90f5`@`qwiklabs-gcp-00-3688114e90f5`.iam.gserviceaccount.com) and and click **Create**.
    
9. To test the trigger, click **Run** and then **Run trigger**.
    
10. Click the **History** link and you should see a build running. Wait for the build to finish, and then click the link to it to see its details.
    
11. Scroll down and look at the logs. The output of the build here is what you would have seen if you were running it on your machine.
    
12. Return to the Artifact Registry service. You should see a new image in the **devops-repo** > **devops-image** folder.
    
13. Return to the **Cloud Shell Code Editor**. Find the file `main.py` in the `gcp-course/devops-repo` folder.
    
14. In the main() function, change the title property to `"Hello Build Trigger."` as shown below:
    

@app.route("/")
def main():
    model = {"title":  "Hello Build Trigger."}
    return render_template("index.html", model=model)

15. Commit the change with the following command:

cd ~/gcp-course/devops-repo
git commit -a -m "Testing Build Trigger"

Copied!

16. Enter the following to push your changes to Cloud Source Repositories:

git push origin main

Copied!

17. Return to the Cloud Console and the **Cloud Build** service. You should see another build running.

Click **Check my progress** to verify the objective.

Assessment Completed!

Automate Builds with Trigger.

Assessment Completed!

## Task 6. Test your build changes

1. When the build completes, click on it to see its details.
    
2. Click **Execution Details**,
    
3. Click the **Image name**. This redirects you to the image page in Artifact Registry.
    
4. At the top of the pane, click **Copy path** next to the image name. You will need this for the next steps. The format will look as follows.
    

us-east4-docker.pkg.dev/qwiklabs-gcp-00-3688114e90f5/devops-demo/devops-image@sha256:8aede81a8b6ba1a90d4d808f509d05ddbb1cee60a50ebcf0cee46e1df9a54810

**Note:** Do not use the image name located in Digest.

5. Go to the **Compute Engine** service. As you did earlier, create a new virtual machine to test this image. Click **DEPLOY CONTAINER** and paste the image you just copied.
    
6. Select **Allow HTTP traffic**.
    
7. When the machine is created, test your change by making a request to the VM's external IP address in your browser. Your new message should be displayed.
    

**Note:** You might have to wait a few minutes after the VM is created for the Docker container to start.

Click **Check my progress** to verify the objective.

Assessment Completed!

Test your Build Changes.

Assessment Completed!
```
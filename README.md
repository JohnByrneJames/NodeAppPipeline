# Documentation of how to set up a CICD pipeline using Jenkins and AWS.

## Requirements

* Git Bash (Linux Terminal)
* AWS running EC2 Instance
* Downloaded/ Cloned this Repository into a folder on your local machine.
* Access to a Jenkins Server
* Access to a EC2 Instance

## What will be achieved?

The Automation of merging working code into the master branch of a version control system (Git) and Deployment of the app onto your EC2 instance. This will be done using Jenkins which is an automation server, it facilitates continuous integration and continuous delivery.

## Step 1 - Set up GitHub Repository

Now that you have downloaded/ cloned this Repository, open your git bash and navigate to the directory where you cloned the repositories documents.

![STEP1_GIF](images/How_to_locate_folder_through_GITBASH.gif)

Once inside the directory inside GitBash. You need to unlink the Remote which will be attached the repository, this will be to this Repository as I linked my own. This objective is to unlink that remote and link your own.
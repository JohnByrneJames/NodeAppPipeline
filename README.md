# Documentation of how to set up a CICD pipeline using Jenkins and AWS.

## Requirements

* Git Bash (Linux Terminal)
* AWS running EC2 Instance
* Downloaded/ Cloned this Repository into a folder on your local machine.
* Access to a Jenkins Server
* Access to a EC2 Instance
* Vagrant, OracleVM and Ruby Downloaded and properly configured on your local System. Find my guide to this [**HERE**](https://github.com/JohnByrneJames/VM_Proxy_Machine)

## What will be achieved?

The Automation of merging working code into the master branch of a version control system (Git) and Deployment of the app onto your EC2 instance. This will be done using Jenkins which is an automation server, it facilitates continuous integration and continuous delivery.

## Step 1 - Set up GitHub Repository

Now that you have downloaded/ cloned this Repository, open your git bash and navigate to the directory where you cloned the repositories documents.

![STEP1_GIF](images/How_to_locate_folder_through_GITBASH.gif)

_To auto complete a word use the `tab` key on your keyboard._

Once inside the directory inside GitBash. You need to unlink the Remote which will be attached to this GitHubs repository. The objective is to unlink that remote and link it to a Repository you have made on your own GitHub.

### Creating a Repository and linking it

**If you need help creating a Repository use the below Dropdown**

<details>
<summary> ❓ How to create a Repository on GitHub ❗ </summary> 
<p>


![GIT](images/GitHub_Step1_1.PNG)

Go to your GitHub and Create a new Repository.

![GIT](images/GitHub_Step1_2.PNG)

Now name the Repository something appropriate so you can refer back to what it is easily in the future.

![GIT](images/GitHub_Step1_3.PNG)

When you create the Repo you can see there are all these instructions, we need the second one as we are using an existing Repo.

Copy the `git remote add origin git@github.com:JohnByrneJames/MyNodeAppPipeline.git`.

</p>
</details>

**If you need help replacing the remote in git use below Dropdown**

<details>
<summary> ❓ How to Replace the Remote | TEXT ❗ </summary>
<p>

**If you haven't already navigate to your directory using Git Bash**

Now go into your git bash and check which remote is currently connected to your Repository.

```bash
git remote --v
```

This will show you the remotes that your git directory is connected to. We need to remove that remote and add the one you have recieved when you created the GitHub Repository.

```bash
git remote rm origin
``` 

By default the remote is called origin, so the last command will remove the connection to that github, if you do `git remote --v` again it should not show anything. Next we are going to add our own remote.

```bash
## - My remote

git remote add origin git@github.com:JohnByrneJames/MyNodeAppPipeline.git

### - If SSH 

git remote add <remote name> <github.com:<GitHubUsername>/<RepositoryName>.git 

### - If HTTP

git remote add <remote name> http://<github.com:<GitHubUsername>/<RepositoryName>.git 
```

Now when you use the `remote --v` command it should now show the Repository you just created as the origin. Now we need to rebase the origin on your Repository by making a push of the contents that should be in your Repository.

```bash
git push -u origin master
```

This will push the contents of your directory the GitHub, the `.gitignore` file will do all the exclusions for you. The `-u` is an optional setting that will set an upstream connection making the origin your default push location. the `origin master` part is the upstream origin, E.G. the GitHubs cloud storage.


Now go back to your GitHub Page and Refresh. You should now see the contents appear in that Github, identical to the screenshot below.

![GIT](images/GitHub_Step1_4.PNG)


</p>
</details>

<details>
<summary> ❓ How to Replace the Remote | VIDEO ❗ </summary> 
<p>

**This is the video, it is a little easier if you are experienced using GitHub**

![STEP1.1_GIF](images/How_to_add_remote_to_gitHub.gif)

</p>
</details>

## Step 2 - Testing VM on local machine

**In order to garuntee this will work, we are going to first test this**

If you haven't then go to this [**REPO**](https://github.com/JohnByrneJames/VM_Proxy_Machine) and follow the documentation to set up your VM, Vagrant and Ruby.

## Step 2.1 - Power Up Machine

All of the VM provisions have been prepared in advance, they are already included and properly configured in this folder. To begin the VM creation process make sure you are inside your directory again in the GitBash terminal. 

The first command to enter is:

```bash
vagrant up
```

This may take a few minutes to set up depending on your machines processing power/ virtualization capabilities...

_It should look something like the image below (this is about a minute in after the app VM has been created)_

![STEP2_GIF](images/VagrantUp_process.gif)

Once that has completed, we need to test the app to see if it working.

## Step 2.1 - Navigate to the App and run it

Now we have started up the Virtual Machines we need to enter the virtual environment and test if the app is working. 


<details>
<summary> ❓ Enter Vagrant and Run NGINX server | TEXT ❗ </summary>
<p>

First lets enter the machine using `SSH`:

```bash
Vagrant SSH app
```

You should now be inside the VM, from here we need to navigate the location where we are going to run the app.

```bash
cd /home/ubuntu/app
```
_**Tip**: If you use the `tab` key it will auto complete the path so `ho` 🠆 `tab-press` 🠆 `home`_

Now we are inside the app folder, run the following commands to install the required dependencies and then run the app so it is listening on port 3000.

```bash
# Install dependencies
sudo npm install

# Test if installed correctly
sudo npm test

# Run App
sudo node app.js
```

_You should get these as your test results_

![Vagrant](images/Step2.NPM_Tests.PNG)

_After running `sudo node app.js` you should get `Your app is ready and listening on port 3000`_

![Vagrant](images/Step2.NPM_Node_Up.PNG)

Now we are able to connect to the NGINX server and view the two web page that we have running from our Virtual Machine.

| Web Page                              | Description                                                                   |
|---------------------------------------|-------------------------------------------------------------------------------|
| http://development.local/             | This is the home-page                                                         |
| http://development.local/fibonacci/10 | This will return fibonacci number at place   in 10 in the fibonacci sequence. |

If this loads then you have successfully completed this step!

</p>
</details>


<details>
<summary> ❓ Enter Vagrant and Run NGINX server | VIDEO ❗ </summary>
<p>

**Videos are easier if you are experienced with Vagrant**

![STEP2_GIF](images/VagrantUp_App.gif)

If you can get the web page to load with the same contents as was shown in this Gif then you have completed this step!

</p>
</details>

**Now that we have confirmed the App is working**

We are going to turn off the VirtualMachines on our local computer to free up processing power. To do use the below commands.

```bash
vagrant halt 
```

This will close both the `app` and `db` virtual machines. If you want to double check they are down the you can use the command:

```bash
vagrant status
```

![Vagrant](images/Step2.Halt_Instances.PNG)

_The virtual machines should appear as `poweroff`._


## Step 3 - Set Up Continuous Integration with Jenkins

**From this point onwards, I am going to be following the process in respect to the Automation server set up at Sparta, these can be altered to most configurations.**

Firstly we need to set up a development branch, this can be easily within the same GitBash terminal we were using before. This will be the branch that we will push our code to, the integration will be done by Jenkins.

Inside your GitBash terminal enter the following commands:

```bash
# Create the branch locally
git checkout -b develop

# Push branch to origin on GitHub (UpStream)
git push --set-upstream origin develop
```

Now when we eventually set up the CI (Continuous Integration) we want to be able to change something that triggers the **Job**. So create a `.md` using:

```bash
# Create file that will change and trigger merge
touch DevBranch.md

# Enter file and make slight change
nano DevBranch.md
```

_I simply added `this was done in the DevBranch`._

_Exit the nano with `Ctrl+s` and `Ctrl+x`_

![Vagrant](images/Step3_Nano_details.PNG)

## Step 3.1 - Set up Jenkins Job for CI (Continuous Integration) and CD (Continuous Delivery)


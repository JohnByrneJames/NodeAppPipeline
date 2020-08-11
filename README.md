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

_To auto complete a word use the `tab` key on your keyboard._

Once inside the directory inside GitBash. You need to unlink the Remote which will be attached to this GitHubs repository. The objective is to unlink that remote and link it to a Repository you have made on your own GitHub.

### Creating a Repository and linking it

**If you need help creating a Repository use the below Dropdown**

<details>
<summary>How to create a Repository on GitHub</summary> 
<pre>


![GIT](images/GitHub_Step1_1.PNG)

Go to your GitHub and Create a new Repository.

![GIT](images/GitHub_Step1_2.PNG)

Now name the Repository something appropriate so you can refer back to what it is easily in the future.

![GIT](images/GitHub_Step1_3.PNG)

When you create the Repo you can see there are all these instructions, we need the second one as we are using an existing Repo.

Copy the `git remote add origin git@github.com:JohnByrneJames/MyNodeAppPipeline.git`.

</pre>
</details>

**If you need help replacing the remote in git use below dropdowns**

<details>
<summary>How to Replace the Remote | TEXT</summary>
<br>
This is how you dropdown.
<br><br>
<pre>
&lt;details&gt;
&lt;summary&gt;How do I dropdown?&lt;&#47;summary&gt;
&lt;br&gt;
This is how you dropdown.
&lt;&#47;details&gt;
</pre>
</details>

<details>
<summary>How to Replace the Remote | VIDEO</summary> 
<br>
This is how you dropdown.
<br><br>
<pre>
&lt;details&gt;
&lt;summary&gt;How do I dropdown?&lt;&#47;summary&gt;
&lt;br&gt;
This is how you dropdown.
&lt;&#47;details&gt;
</pre>
</details>
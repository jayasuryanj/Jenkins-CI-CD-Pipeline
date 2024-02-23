# Jenkins-CI-CD-Pipeline
Jenkins CI/CD Pipeline
What is Jenkins?
Jenkins is a smart tool for developers to help build, test, and launch their code software easily and consistently in a CI/CD pipeline. It’s pivotal open-source automation instrument vital to the realm of DevOps and the construction of robust CI/CD pipelines. It empowers developers with the capability to consistently construct, validate, and launch their software products. Jenkins holds its place as one of the most influential tools in the CI/CD domain, offering a master-slave architecture that facilitates distributed builds.

The master node serves as the orchestrator, overseeing GUI operations and API access points, while the agents, interchangeably referred to as workers or slaves, carry out the actual workload. In essence, Jenkins serves as the cornerstone. Streamlining software development and deployment processes, enhancing reliability, and expediting the delivery of high-quality software solutions.

Think of it as a traffic cop directing the flow of software changes. It ensures that everything runs smoothly by coordinating the work of different parts of the software team. it is the go-to helper for developers, making their lives easier and software better.

Installation and Setup
First thing we will do is set up Jenkins through a Docker container. We will make this straightforward so we can move on to the essential parts of Jenkins. Before we start with process, be sure to install docker on your local host.

Lets get the official Jenkins image and start the Jenkins server. Host it over your IDE.
#fetch jenkins image to localhost
$ docker pull jenkins/jenkins
#Let's run docker container with pulled jenkins image
$ docker run -d -p 8080:8080 -v jenkins_volume:/var/jenkins_home — name my_jenkins jenkins/jenkins
Before we proceed, let me break down the command above for setting up Jenkins within a Docker container:

docker run: This command initiates the container.
-d: It operates in detached mode, meaning you won't see any logs directly.
-p 8080:8080: This line exports a port for external access.
-v: The "v" signifies volume, where all Jenkins files are stored. This step is crucial because, without it, your configuration and setup could be lost if the container is terminated or experiences issues. For more details on Docker volumes, refer to Docker Volume documentation.
jenkins_volume: This points to the directory on your local host.
:/var/jenkins_home: It links to the directory within the Docker container.
--name my_jenkins: This is an optional container name, but it's considered a good practice to provide a name for the container.
jenkins/jenkins: Finally, this indicates the name of the image.
Once this setup is complete, if you open your web browser and navigate to localhost:8080, you should see the expected interface.


You need the password to login the Jenkins. Jenkins provides an initial password by starting the server. This can be found:

Checking logs → docker logs [container name] or
interaction with container and see under given path /var/jenkins_home/secrets/initialAdminPassword
I will do with the second way

#this is my initial password
$ docker exec my_jenkins cat/var/jenkins_home/secrets/initialAdminPassword
7805a2621fee4388bbc77e00b5655ad7
Then click continue and next step install preferred plugins. It will take some minutes. After plugins are installed you should see Admin panel where you may set your name and password.


Set your username, password and then save it to continue. The next screen should show your Jenkins dashboard.


Exploring Jenkins Pipelines
So we will go over on how many pipelines there are available. in Jenkins, there are many projects: Maven, Projects Pipeline, External Job, multi-configuration Project Folder Multi, Branch and Organization folder.

Yet, the most commonly used pipeline types are single pipeline and the multi branch pipeline. Freestyle projects are rarely used.

so what is the freestyle pipeline? Freestyle is a simple project where you can configure everything and it Jenkins will build your project and you can use any SCM (source code management). If you want to deploy something, you want to execute something. However, the majority of the pipelines are based on CI/CD pipeline project.

It basically runs on an agent. An agent is basically executor which execute all of your steps. I will show you some agents and also the pipelines, and this only works on a single branch of your github. Reposition, let’s say you have a product

What is a Single Pipeline?


Example of Single Pipeline
A single pipeline in Jenkins is designed to build, test, and deploy one specific application or project. Let’s go over the essentials and advantages

Use Case: It’s ideal when you have a single software project that you want to automate the build and deployment process for.
Configuration: You create one pipeline job in Jenkins, and it handles the entire lifecycle of that single project.
Advantages: Simple to set up and manage for smaller projects. It streamlines the automation for a single codebase.
What is a Multibranch Pipeline:


Example of Multibranch Pipeline
A multibranch pipeline is used when you have multiple branches or versions of your software project, and you want to automate the CI/CD process for each branch independently.

Use Case: It’s perfect for larger projects with multiple development branches (e.g., feature branches, development, staging, production).
Configuration: You configure a single multibranch pipeline job, and it automatically detects and manages branches in your source code repository.
Advantages: Enables automation for various branches simultaneously. It helps ensure that each branch is tested and deployed separately, maintaining code quality and stability.


This is what a Single Pipeline configuration looks like
Lets create pipeline on Multibranch

Insert Display name then Branch source credentials.
Copy Github URL and push onto your IDE

Go to your repository to view code

Copy and paste code into your pipeline credentials

Ensure that your Github credentials are validated

1. Strategy-All branches, 2.Discover pull request- Both current request 3. Delete forks section not required

Property Strategy- All branches →Build Configure:Mode-Jenkinfile

Ensure days to keep old files are 3 then the max # of old items to keep 5–7 days.
After that your pipeline is created. We will go to scan repository to view our status.


Then we will create new branch.


we will use git versioning to push the code to new branch
So if you head back to your Jenkins repository, you will click scan repository and you will see production repository.


Before you click on Scan Repository

After you refresh your Scan Repository Screen

Enter your branch and see it being build(the #1 representing the branch being built)
We can also create 2 or more stages within the pipeline while a branch is being built.


Let’s create changes to the production branch and push it.
To ensure that our changes are in our repository. Let’s go to our Github.


Go to branches. Click on production

Here it is!
Go back to Jenkins and click on build now section to see branch production.


Before deployment changes

After deployment changes
After we approve changes, we will go to pull request to merge the changes on to our main branch.


Go to Open Pull Request

Confirm merge


We confirm that the changes are installed within our main branch.

Before

After
The purpose of multibranch pipelines is for everybody to have its own branch or maybe like a group of team. A group of people or maybe a team can do some changes, or they can have separate testings for their changes rather than creating one every time, a PR and then merge, those changes to a main branch and then start a single branch pipeline. You can also do multi branch and have a separate branch and test your changes. It will not affect other branches.

Single VS Multi Branch Pipelines- Conclusion
The main distinction between single and multi-branch pipelines in Jenkins lies in their branch-handling capabilities. Single branch pipelines are limited to building changes from a single branch, making them less suitable for repositories with multiple ongoing branches. On the other hand, multi-branch pipelines provide a dynamic and efficient solution. They automatically detect and build changes from all branches within a GitHub repository, offering separate CI/CD pipelines for each branch as needed. This versatility streamlines the development process, making it easier to manage and test code changes across various branches, all within a single, consolidated pipeline. With multi-branch pipelines, you can enhance your Jenkins workflow by efficiently handling multiple branches and their respective CI/CD requirements.

To sum it up, Jenkins is your factory manager in the world of software development. It’s a tool that helps automate and streamline the process of building, testing, and deploying code. But the real magic happens when you dig into its pipelines. These pipelines are like the assembly lines in a factory, where you can define each step of your software’s journey, from writing code to delivering it to users. Mastering Jenkins pipelines is crucial for DevOps because it allows you to create a well-organized and automated workflow, making your software development process faster, more reliable, and less error-prone. It’s the secret that keeps modern development teams running smoothly. So, if you’re diving into DevOps, Jenkins and its pipelines are your best friends on this exciting journey.

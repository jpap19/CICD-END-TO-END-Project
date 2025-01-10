<h1>CICD-END-TO-END-Project In Production Environment</h1>

<h2>Description</h2>
In this project , We will implement an end to end CICD solution on AWS using code build and code pipeline.
<br />

<h2>Resources And Links Required:</h2>

- <b>docs.aws.amazon.com</b> https://docs.aws.amazon.com/whitepapers/latest/cicd_for_5g_networks_on_aws/cicd-on-aws.html

<h2>Project walk-through Steps:</h2>

The implementation will be in done in 4 Steps:

STEP 1:  Upload a simple python application to a github repository.

STEP 2:  Configure AWS CodeBuild.

STEP 3:  Create an AWS CodePipeline.

STEP 4: Trigger the CI Process

STEP 5: Deploy the application on a virtual machine using Codedeploy

STEP 6: Integration of CodeDeply Stage in our Pipeline.

This is the high level overview of what we are going to setup in this Project Lab: We will be using codepipeline as an orchestrotor to invoque codebuild to build our application and push it to dockerhub, 
Then invoque codeploy to deploy the application on a virtual machine

<h2>Project high level overview :</h2>
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/project%20overview.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
<p align="center">

This diagram provides an overview of the resources included in this project. We will implement an end to end CICD solution on AWS using codebuild and codepipeline and codedeploy. We will use github as repositores source instead of code commnit..

<h2>Project  Implementation:</h2>
 
STEP 1:   Upload a simple python application to a github repository.

The first step in our CI project is to set up a GitHub repository to store our Python application's source code. We will create a new repository on GitHub  following these steps:
sign in to our account from github.com , Click on the "+" button in the top-right corner and select "New repository, Give our repository a name (simple-python-app) and an optional description, Choose the appropriate visibility option based on our needs, 
Initialize the repository with a README file, Click on the "Create repository" button to create the new GitHub repository.

1.1 Simple Python Application Uploaded to a github repository: <br/>

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

 2.1 Creation of the launch template: <br/>

2.1.1 From the EC2 dasboard, select launch template, provide a name, Choose the OS type.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 2:  Creation and Configuration of our AWS CodeBuild project.

In this step, we'll create our AWS CodeBuild project  and  configure it to build our Python application based on the specifications we define. CodeBuild will take care of building and packaging our application for deployment. 

2.1 Creation of the build project: <br/>

In the AWS Management Console, navigate to the AWS CodeBuild service, Click on the "Create build project" button, Provide a name (simple-python-flask-service) for our build project.
For the source provider, choose "AWS CodePipeline." we will choose Github as our repository source.

2.1.1 AWS CodeBuild project creation step 1:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.1.2 AWS CodeBuild project creation step 2:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.1.3 AWS CodeBuild project creation step 3:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.1.4 AWS CodeBuild project created:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.2 AWS System manager configuration to store Docker credentials that are sensitive informations: <br/>

In AWS console, search for system manager and choose parameter store, then create 3 parameters for username, password and the url that will be use in our build spec file to connect Docker hub. Those parameters should matched what is specified in the buildspec file
as follow :  /myapp/docker-credentials/username, /myapp/docker-credentials/password, /myapp/docker-registry/url.

2.2.1 All 3 parameters  created in AWS system manager:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3 Starting the building process: <br/>

From the aws code build project, select our project and click on start build, we should encounter a failure due to permission missing in our code build service role:

2.3.1 Code build failed due to permission denied, caused by missing policy.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3.2 Code build failed due to permission denied due to ....
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3.3 Code build failed due to permission denied due to ....
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3.4 Code build successful after troubleshooting service role policy added, environment privilege enabled.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

2.3.4 Image pushed to docker hub after project succefull in codebuild.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 3:  Creation of our AWS CodePipeline.

In this step, we'll create an AWS CodePipeline to automate the continuous integration process for our Python application. AWS CodePipeline will orchestrate the flow of changes from our GitHub repository to the deployment of our application.

3.1 AWS Codepipeline creation process for our CICD Project: <br/>

From the AWS Management Console, we will navigate to the AWS CodePipeline service, Click on the "Create pipeline" button, Provide a name for our pipeline and click on the "Next" button, 

3.1.1 Creation of our AWS CodePipeline Step 1.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

For the source stage, select "GitHub" as the source provider, Connect your GitHub account to AWS CodePipeline and select our repository, Choose the branch we want to use for our pipeline,

3.1.2 Creation of our AWS CodePipeline Step 2.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

 In the build stage, select "AWS CodeBuild" as the build provider, and select our CodeBuild project we created prewviously "simple-python-app"

3.1.3 Creation of our AWS CodePipeline Step 3.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Continue configuring the pipeline stages, such as deploying your application using AWS Elastic Beanstalk or any other suitable deployment option. Review the pipeline configuration and click on the "Create pipeline" button to create your AWS CodePipeline.

3.1.4 AWS codepipeline created.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />


STEP 4: Trigger the CI Process.

From our github repository, let navigate to the simple python application and simply add a space at the end of the source code as shown below. commit the change.

4.1 Modification in the python app code source: <br/>

3.1.1 From the EC2 dasboard, select launch template, provide a name, Choose the OS type.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

As soon as the change is made in the python source code from our github account, it will trigger the pipeline to start executing

4.1.1 The modification made has triggerred our pipeline to start.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Then it will complete the build.

4.1.2 Pipeline executed after change in the python app code source.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

An new image will be pushed to our docker hub account.

4.1.2 Pipeline executed after change in the python app code source.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 5:  Deploy the application on a virtual machine using Codedeploy
 From the AWS Web console, navigate to codedeploy and click on create application. 

5.1 Application created in codedeploy.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
 
 After creating the application in codedeploy, we need to created the virtual machine on which the application will be deployed. we will make sure we add a Tag name that will be used by codedeploy.

5.2 Virtual machine (EC2 instance) created with Tags name.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

We need to install a codeploy agent on our virtual machine. for that we need to login into the EC2 instance.

5.2.1 Login succesful to the Virtual machine (EC2 instance).
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

We will follow the documentation on how to Install the CodeDeploy agent for Ubuntu Server. for that we need to run the following commands:

sudo apt update
sudo apt install ruby-full
sudo apt install wget
cd /home/ubuntu
wget https://bucket-name.s3.region-identifier.amazonaws.com/latest/install  
(for us it will be: wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install)
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent status

5.2.1 Steps to Install the CodeDeploy agent for Ubuntu Server
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.2.2 Agent installation on the EC2 instance step 1.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.2.2 Agent installed on the EC2 instance and service started.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.3 Codeploy Agent installed on the Virtual machine (EC2 instance).
Now we need to create a role and assign it to our ec2 instance

5.3.1 Codedeploy role created and assign to the ec2-instance.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Now we need to retart the codedeploy agent service on our virtual machine by running the command "sudo service codedeploy-agent restart"
5.3.2 Agent service on our virtual machine.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5 CodeDeploy Configuration

At this stage, we created our codeploy and iam service role for it; created a virtual machine, installed a codedploy agent onto it and attached the iam role to it.
Now we need to setup the connection between codeploy and the virtual machine (EC2-instance). Before that we will need to add EC2-full access to the ec2-codeploy role we created.
From the codeDeploy console, choose the application we created, and create a deployment group, and provide the arn of the service role we created.

5.5.1 EC2-full access added to the ec2-codeploy role
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.1 CodeDeploy DeploymentGroup creation step 1
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.2 CodeDeploy DeploymentGroup creation step 2
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.3 CodeDeploy DeploymentGroup creation failure Observed

5.5.3.1 CodeDeploy DeploymentGroup creation step 3 (creation failed due to assume role mising.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.3.2 Add the trust policy to allow AWS Code Deploy service to assume this role. 
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.3.3 CodeDeploy DeploymentGroup created
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.4 Deployment creation Implementation

Now that the codeDeploy configuration is complete, we need to create the deployment itself
From codedeploy application, select our python application and go to the deployment tab, select the deployment group and connect to our Github account where the repository of the appplication code source is stored.

5.5.4.1 Deployment creation for our application from codeDeploy step 1
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Then provide the repository name and reference the commit ID from that repository on our github account

5.5.4.2 Deployment creation for our application from codeDeploy step 2
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
The remaining options are not needed at this time, simple click on create to finalize our deployemnt creation

5.5.4.3 Deployment creation in progress: Successfully connect to our github
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

After successfully connecting to the github repository, our deployment will fail. The reason is because it not able to fetch the application source code file.
To fix that, the appspec.yml file of our python application needs to be put at the root level of the github directory as shown below:

5.5.4.4 Python app files put at the root level of the github
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

The appspec.yml file will run our application, It will connect to our dockerhub account to pull the image using the start_container.sh file as showing below: for that we will need to install docker on our virtual machine
Docker will be installed on the vm using the command: sudo apt install docker.io -y.

5.5.4.5 Deployment succesfull completed:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.5 CodePipeline Integration with our Codeploy

We need to go our pipeline created earlier, click on edit and  then add a stage for codeploy, reference the deployment group and our python application name.

5.5.5.1 Adding codedploy stage to our pipeline step 1
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.5.1 Adding codedploy stage to our pipeline step 2
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

Now let make a modification to one of our file ( the start_container.sh): add a space

5.5.5.1 Adding a space to our start_container.sh file and commiting the change in our github account:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

The change made(adding just a space in the file above) will trigger the pipeline and continue until running the codedploy stage

5.5.5.2 Codedeploy stage has triggered our pipeline to run:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

The codedeploy stage has failed after the build was successfull and the image pushed to dockerhub.

5.5.5.2 Codedeploy stage has failed during the pipeline to running: Not permission to access S3
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

 We need to add the S3 Full access permission to our codepipeline role

 5.5.5.2 S3 Full access permission to our codepipeline role
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

5.5.5.3 Codedeploy stage has been run and completed in our pipeline:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 6: Integration of CodeDeply Stage in our Pipeline.

Now from our pipeline, we need to add a stage called "CodeDeploy", make a modification to our source code and watch the the whole pipeline triggered and complete until deployment.

6.1 Adding Codedeploy stage to our pipeline Step 1:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

6.1 Adding Codedeploy stage to our pipeline Step 2:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

6.1 The entire pipeline triggered and completed until deployment Step 1:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

6.1 The entire pipeline triggered and completed until deployment Step 2:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />


CONCLUSION: <br/>

In this activity, we have implemented an end to end CICD AWS solution, using Github as code source provider, Code pipeline as our orchestrator to imvoque Code build for our buiding stage, push the image to our dockerhub account.

Code pipeline also invoque code deploy to deploy the python application onto a virtual machine (EC2 server). The whole process was successfull.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

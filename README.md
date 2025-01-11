<h1>CICD-END-TO-END-Project In Production Environment</h1>

<h2>Description</h2>
In this project , We will implement an end to end CICD solution on AWS using code build, code pipeline and code deploy.
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

STEP 6: Integrate of Code Deploy Stage in our Pipeline.

This is the high level overview of what we are going to setup in this Project Lab: We will be using codepipeline as an orchestrotor to invoque codebuild to build our application and push it to dockerhub, 
Then invoque codeploy to deploy the application on a virtual machine

<h2>Project high level overview :</h2>
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/project%20overview.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
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

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python-app.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />


STEP 2:  Creation and Configuration of our AWS CodeBuild project.

In this step, we'll create our AWS CodeBuild project  and  configure it to build our Python application based on the specifications we define. CodeBuild will take care of building and packaging our application for deployment. 

2.1 Creation of the build project: <br/>

In the AWS Management Console, navigate to the AWS CodeBuild service, Click on the "Create build project" button, Provide a name (simple-python-flask-service) for our build project.
For the source provider, choose "AWS CodePipeline." we will choose Github as our repository source.

2.1.1 AWS CodeBuild project creation step 1:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codebuild1.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.1.2 AWS CodeBuild project creation step 2:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codebuild2.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.1.3 AWS CodeBuild project creation step 3:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codebuild3.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.1.4 AWS CodeBuild project created:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codebuild8.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.2 AWS System manager configuration to store Docker credentials that are sensitive informations: <br/>

In AWS console, search for system manager and choose parameter store, then create 3 parameters for username, password and the url that will be use in our build spec file to connect Docker hub. Those parameters should matched what is specified in the buildspec file
as follow :  /myapp/docker-credentials/username, /myapp/docker-credentials/password, /myapp/docker-registry/url.

2.2.1 Docker's credential parameters configure in our buildspec.yml in file:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/DockerConnectionParameters.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.2.2 All 3 parameters  created in parameter store in AWS system manager:

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/parameter%20store.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.3 Starting the building process: <br/>

From the aws code build project, select our project and click on start build, we should encounter a failure due to permission missing in our code build service role:

2.3.1 Code build failed due to permission denied, caused by missing policy.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codebuild%20failed.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.3.2 Code build failed due environment privilege enabled needed.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codebuild%20failed4_DockerDaemonPermission%20needed.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.3.3 Code build process starting and continuing running through after troubleshooting service role policy added, environment privilege enanled.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/building%20process1.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.3.4 Code build successful 1.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/building%20process%20successful1.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.3.5 Code build successful 2.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/building%20process%20successful2.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

2.3.6 Image pushed to docker hub after project succefull in codebuild.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/SimplePuthonApp_pushed%20to%20DockerHub.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

STEP 3:  Creation of our AWS CodePipeline.

In this step, we'll create an AWS CodePipeline to automate the continuous integration process for our Python application. AWS CodePipeline will orchestrate the flow of changes from our GitHub repository to the deployment of our application.

3.1 AWS Codepipeline creation process for our CICD Project: <br/>

From the AWS Management Console, we will navigate to the AWS CodePipeline service, Click on the "Create pipeline" button, Provide a name for our pipeline and click on the "Next" button, 

3.1.1 Creation of our AWS CodePipeline Step 1.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codepipeline%20creation%20step%201.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

For the source stage, select "GitHub" as the source provider, Connect your GitHub account to AWS CodePipeline and select our repository, Choose the branch we want to use for our pipeline,

3.1.2 Creation of our AWS CodePipeline Step 2.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codepipeline%20creation%20step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

 In the build stage, select "AWS CodeBuild" as the build provider, and select our CodeBuild project we created prewviously "simple-python-app"

3.1.3 Creation of our AWS CodePipeline Step 3.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codepipeline%20creation%20step%203.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

Continue configuring the pipeline stages, such as deploying your application using AWS Elastic Beanstalk or any other suitable deployment option. Review the pipeline configuration and click on the "Create pipeline" button to create your AWS CodePipeline.

3.1.4 AWS codepipeline created and running ongoing.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codepipeline%20creation%20step%204.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

3.1.5 AWS codepipeline cration and running process completed.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codepipeline%20creation%20step%205.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

3.1.6 AWS codepipeline cration and running process completed. 
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codepipeline%20creation%20step%206.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />


STEP 4: Trigger the CI Process.

From our github repository, let navigate to the simple python application and simply add a space at the end of the source code as shown below. commit the change.

4.1.1 4.1 Modification in the python app code source: <br/>

<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/triggerprocess1.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

As soon as the change is made in the python source code from our github account, it will trigger the pipeline to start executing

4.1.1 The modification made has triggerred our pipeline to start.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/triggerprocess2.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

Then it will complete the build.

4.1.2 Build Stage is still ongoing after the modification made in github has triggered the pipeline to run.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/triggerprocess3.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

4.1.3 Pipeline executed after change in the python app code source.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/triggerprocess4.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

An new image will be pushed to our docker hub account.

4.1.2 New Image push to docker hub after Pipeline executed following the change made on the python app code source from Github.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/triggerprocess5.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

STEP 5:  Deploy the application on a virtual machine using Codedeploy

 From the AWS Web console, navigate to codedeploy and click on create application. 

5.1 Application creation in codedeploy.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codedeploy%20name.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

An IAM role will be need for our code deploy execution

5.1.1 Creation of Codedeploy role with ec2-instance full access.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/ec2FullAccessAddedTo%20codeploy%20Iam%20Role.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />
 
 After creating the application in codedeploy, we need to created the virtual machine on which the application will be deployed. we will make sure we add a Tag name that will be used by codedeploy.

5.2 Virtual machine (EC2 instance) created with Tags name.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/ec2Tags.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

We need to install a codeploy agent on our virtual machine. for that we need to login into the EC2 instance.

5.2.1 Login succesful to the Virtual machine (EC2 instance).
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/ec2%20connection2.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.2  Installation of CodeDeploy agent for Ubuntu Server

We will follow the documentation on how to Install the CodeDeploy agent for Ubuntu Server. for that we need to run the following commands one at a time:

sudo apt update - sudo apt install ruby-full - sudo apt install wget - cd /home/ubuntu - wget https://bucket-name.s3.region-identifier.amazonaws.com/latest/install  
(for us it will be: wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/latest/install)- chmod +x ./install - sudo ./install auto - sudo service codedeploy-agent status

5.2.1 Agent installation on the EC2 instance step 1.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codeploy%20agent%20step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.2.2 Agent installed on the EC2 instance and service started.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/codeploy%20agent%20installed.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.3 Creation of Code deploy role assigned to the virtual machine EC2

Now we need to specifically link (assign) the code deploy role we created here above to our ec2 instance.  From the EC2 instance console, we will select our instance, click on action, security, 
choose modify IAM role and then select the code deploy role created previously and assigned it.

5.3.2 Codedeploy role assigned to our ec2-instance.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/ec2-codeploy-role.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

In order for our role modification to take effect, we need to retart the codedeploy agent service on our virtual machine by running the command "sudo service codedeploy-agent restart"

5.3.2 Code deploy agent running after service restart, following the role attached to our virtual machine ec2.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/AgentRestartOnEc2after%20role%20attached.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5 CodeDeploy Configuration

At this stage, we created our codeploy and iam service role for it; created a virtual machine, installed a codedploy agent onto it and attached the iam role to it.
Now we need to setup the connection between codeploy and the virtual machine (EC2-instance). Before that we will need to add EC2-full access to the ec2-codeploy role we created.
From the codeDeploy console, choose the application we created, and create a deployment group, and provide the arn of the service role we created.

5.5.1 EC2-full access added to the ec2-codeploy role
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/ec2FullAccessAddedTo%20codeploy%20Iam%20Role.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.1 CodeDeploy DeploymentGroup creation step 1
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/CodeDeploy%20DeploymentGroup%20creation%20step%201.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.2 CodeDeploy DeploymentGroup creation step 2
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/CodeDeploy%20DeploymentGroup%20creation%20step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.3 CodeDeploy DeploymentGroup creation failure Observed

5.5.3.1 CodeDeploy DeploymentGroup creation step 3 (creation failed due to assume role mising.
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/CodeDeploy%20DeploymentGroup%20creation%20step%203.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.3.2 Adding the trust policy to allow AWS Code Deploy service to assume this role. 
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Add%20the%20trust%20policy%20to%20allow%20AWS%20Code%20Deploy%20service%20to%20assume%20this%20role..png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.3.3 CodeDeploy DeploymentGroup created
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/CodeDeploy%20DeploymentGroup%20created.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.4 Deployment creation Implementation

Now that the codeDeploy configuration is complete, we need to create the deployment itself
From codedeploy application, select our python application and go to the deployment tab, select the deployment group and connect to our Github account where the repository of the appplication code source is stored.

5.5.4.1 Deployment creation for our application from codeDeploy step 1
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Deployment%20creation%20for%20our%20application%20from%20codeDeploy%20step%201.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

Then provide the repository name and reference the commit ID from that repository on our github account

5.5.4.2 Deployment creation for our application from codeDeploy step 2
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Deployment%20creation%20for%20our%20application%20from%20codeDeploy%20step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

The remaining options are not needed at this time, simple click on create to finalize our deployemnt creation
After successfully connecting to the github repository, our deployment will fail. The reason is because it not able to fetch the application source code file.

5.5.4.3 Deployment creation for our application from codeDeploy step 2
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Deployment%20creation%20is%20failing%20after%20connecting%20to%20the%20github%20account.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

To fix that, the appspec.yml file of our python application needs to be put at the root level of the github directory as shown below:

5.5.4.4 Python app files put at the root level of the github
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/python%20File%20put%20at%20the%20root%20level.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

The appspec.yml file will run our application, It will connect to our dockerhub account to pull the image using the start_container.sh file as showing below: for that we will need to install docker on our virtual machine
Docker will be installed on the vm using the command: sudo apt install docker.io -y.

5.5.4.5 Deployment succesfull completed:
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/deployment%20successful.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

5.5.4.6 Deployment succesfull completed:
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/deployment%20successful%201.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

STEP 6: Integration of CodeDeply Stage in our Pipeline.

Now from our pipeline, we need to add a stage called "CodeDeploy", make a modification to our source code and watch the whole pipeline triggers and completes until deployment.
We need to go our pipeline created earlier, click on edit and  then add a stage for codeploy, reference the deployment group and our python application name.

6.1 Adding Codedeploy stage to our pipeline Step 1:
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Adding%20Codedeploy%20stage%20to%20our%20pipeline%20Step%201.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

6.2 Adding Codedeploy stage to our pipeline Step 2:
<img src="hhttps://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Adding%20Codedeploy%20stage%20to%20our%20pipeline%20Step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

Now let make a modification to one of our file ( the start_container.sh): we will a space in that file and commit the change in our github account

The change made(adding just a space in the file above) will trigger the pipeline and continue until running the codedploy stage

6.3 The change made to the code source has triggered our pipeline to run as show below :
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Codedeploy%20stage%20has%20triggered%20our%20pipeline%20to%20run.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

The codedeploy stage has failed after the build was successfull and the image pushed to dockerhub.

6.4 Codedeploy stage has failed during the pipeline running: Not permission to access S3
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Adding%20codedploy%20stage%20to%20our%20pipeline%20step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

 We need to add the S3 Full access permission to our codepipeline role

6.5 S3 Full access permission to our codepipeline role
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/s3Full%20Access%20Permission%20added.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

After adding s3 permission, we have release the change for our pipeline to run again

6.6 Codedeploy stage has been run and completed in our pipeline:
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/The%20entire%20pipeline%20triggered%20and%20completed%20until%20deployment%20Step%201.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

6.7 The entire pipeline triggered and completed until deployment Step 1:
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
<br />
<br />

6.8 The entire pipeline triggered and completed until deployment Step 2:
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/The%20entire%20pipeline%20triggered%20and%20completed%20until%20deployment%20Step%202.png" height="150%" width="150%" alt="CICD-END-TO-END-Project"/>
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

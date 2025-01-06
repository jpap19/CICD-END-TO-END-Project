<h1>CI-END-TO-END-Project In Production Environment</h1>

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

<h2>Project high level overview :</h2>

This is the high level overview of what we are going to setup in this Project Lab:
<img src="https://github.com/jpap19/CICD-END-TO-END-Project/blob/main/Screenshots/Design1.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />
<p align="center">

This diagram provides an overview of the resources included in this project. We will implement an end to end CI solution on AWS using code build and code pipeline. We will use github as repositores source instead of code commnit..

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

CONCLUSION: <br/>

In this activity, we have set up a home lab using Elastic SIEM and a Kali VM. We forwarded data from the Kali VM to the SIEM using the Elastic Beats agent, generated security events on the Kali VM using Nmap, and queried and analyzed the logs in the SIEM using the Elastic web interface. We also created a dashboard to visualize security events and then created an alert to detect security events.


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

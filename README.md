# 

<h1>CI-END-TO-END-Project In Production Environment</h1>

<h2>Description</h2>
In this project , We will implement an end to end CI solution on AWS using code build and code pipeline.
<br />

<h2>Resources And Links Required:</h2>

- <b>docs.aws.amazon.com</b> https://docs.aws.amazon.com/whitepapers/latest/cicd_for_5g_networks_on_aws/cicd-on-aws.html

<h2>Project walk-through Steps:</h2>

The implementation will be in done in 4 Steps:

STEP 1:  Upload a simple python application to a github repository.

STEP 2:  Create an AWS CodePipeline.

STEP 3:  Configure AWS CodeBuild.

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

STEP 2:  Create an AWS CodePipeline.

Auto sclaiing group required a launch template for its creation. Let create a launch template with security group where we will open ssh port and 8000 (for the python application), accessible from anywhere.

 2.1 Creation of the launch template: <br/>

2.1.1 From the EC2 dasboard, select launch template, provide a name, Choose the OS type.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 3:  Configure AWS CodeBuild.

 2.1 Creation of the launch template: <br/>

2.1.1 From the EC2 dasboard, select launch template, provide a name, Choose the OS type.
<img src="https://github.com/jpap19/VPC-Project-In-Production/blob/main/Images/launch%20Template2.png" height="150%" width="150%" alt="Nessus Essential Home Lab"/>
<br />
<br />

STEP 4: Trigger the CI Process.

 2.1 Creation of the launch template: <br/>

2.1.1 From the EC2 dasboard, select launch template, provide a name, Choose the OS type.
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

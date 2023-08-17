<p align="center">
<img src="https://github.com/kura-labs-org/kuralabs_deployment_1/blob/main/Kuralogo.png">
</p>
<h1 align="center">Deployment-1.1:  Run a Jenkisn Build and </p> Manually Deploy to Elastic Beanstalk<h1> 

## Step #1 Plan Deployment

![Plan](Deployment_01.1_Pipeline.jpg)

## Step #2 Upload Repository to GitHub

Upload repository to GitHub and generate GitHub token

## Step #3 and Step #4:  Use Jenkins to Auto Build and Auto Test Application

Log into Jenkins create a build Annie_L_1.1 for the application from GitHub Repository https://github.com/LamAnnieV/Deployment_01.1.git and run the build

### Results

![Build](D01.1_Jenkins_Results.jpg)

## Step #5:  Download Repository from GitHub

Download Repository, Unzip files and re-zip files

## Step #6:  Deploy Application on AWS ELASTIC BEANSTALK

**Create EBS Role**

-     AWS/IAM/Roles/Create Role/Select:  AWS Service/[Use Case] Use Cases for other AWS services:  Elastic Beanstalk/Select:  Elastic Beanstalk - Customizable/Next
-     Next
-     Role Name:  aws-elasticbeanstalk-service-role/Create Role

**Create EC2 Role**

-     AWS/IAM/Roles/Create Role/Select:  AWS Service/[Use Case] Select:  EC2/Next
-     [Permissions Policies] Select:   "AWSElasticBeanstalkWebTier" & “AWSElasticBeanstalkWorkerTier”/Next
-     Role Name:  Elastic-EC2/Create Role

**Deploy application in AWS EC2 and Elastic Beanstalk**

-     AWS/Elastic Beanstalk/Environments/Create Environment/Application Name:/[Platform-4] Platform:  Python/Platform Branch:  Python 3.9 running on 64bit Amazon Linux 2023/Select:  Upload Your code/Version Label:  v#/Select: Local File/Choose File:  {files that was downloaded from GitHub, Unzipped, then rezipped}/Next
-     [EC2 instance profile] Select:  Elastic-EC2/Next
-     [Virtual Private Cloud] Select:  default VPC/[Instance Subnets] Select:  us-east-1a/Next
-     [Instances] Root Volume Type:  General Purpose (SSD)/Size:  10/[Capacity] Instance Types:  Deselect all & Select t2.micro/Next
-     Next
-     Submit
**1st Attempt: Health Status:  Degraded**

**Debugging Process:**
-           1. Downloaded 1st 100 lines of the log
-           2. Searched for "Error" in the log and it is contained in the "/var/log/web.stdout.log" section
-           3. Copied "/var/log/web.stdout.log" section to ChatGPT to Explain AWS Log:
-           4. Per ChatGPT, main issue is:  ModuleNotFoundError: No module named 'application...Make sure the application module is correctly named"
-           5. *Renamed the app.py to application.py, rezip content
-           6. Reload Files and Re-Deployed Application on AWS Elastic Beanstalk
**2nd Attempt: Health Status:  Ok**

![AWS](D01.1_AWS_Results.jpg)
  
## Step #7:  Launch Website

http://url-shortener-env.eba-av38k5ye.us-east-1.elasticbeanstalk.com/

![Website](D1.01_Website_Result.jpg)

## *Note for Step #6 Debugging process #5

-  This change should also be made in the GitHub repository.  If this change is made in the repository, this will cause an issue in the Test Stage of the Jenkins Build.  Since the Test Stage imports an object called app from module app.py and that module app.py can no longer be found. In order to resolve this, the code in test_app.py needs to be updated from 'from app import app' to 'from application import app'.  When there are any changes, we need to keep in mind if that change will affect other areas in the pipeline.
            

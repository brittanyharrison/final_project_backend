# Engineering 89: Final Project 
![logo](img/logos/job_centre_logo.png)

## Introduction 

This repository details the processes required to setup and run the [jobcentre++ web application](https://github.com/engineering89-final-project/jcpp). 

## Project Scope  
![img](img/ci_cd_diagram.png)

The aim of this project is to provide a End to End automation pipeline for the jobcentre++ web application. This included:

- Test Driven Development(TDD) for the app
- Continuous Integration using Jenkins
- Continuous Delivery and Deployment with AWS EC2 
- Containerisation and Deployment with Docker 
- Data Persistency using S3 buckets
- Cloud Monitoring with CloudWatch

For detailed documentation on each step to achieve this End to End pipline, oplease refer to the [wiki](https://github.com/brittanyharrison/final_project_backend/wiki) of this repository.  

## Team members:

**Front-end**:
- [Aman](https://github.com/Ahhhh-man) 
- [Mad](https://github.com/monotiller)
- [Niki](https://github.com/NikiNikiforidi)
- [Saim](https://github.com/saim22r)
- [Tom](https://github.com/twilliams9397)

**Backend**:
- [Brittany](https://github.com/brittanyharrison)
- [Dini](https://github.com/DiniH1)
- [Mueed](https://github.com/mueed-shah)
- [Prathima](https://github.com/prathimaautomation)
- [Salem](https://github.com/SBenkhelfaSparta) 

**Automation**:
- [Connor](https://github.com/connorHayler)
- [Filipe](https://github.com/Filipe-Seixas) 
- [Ray](https://github.com/RayWLMo)
- [Ron](https://github.com/rurbonas)
- [Shervin](https://github.com/S-ghanbary98) 

## Software and Tools

<!-- ALL-TOPICS-LIST:START -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<center>
<table>
  <tr>
    <td align="center"><a href="#jenkins"><img src="img/logos/Jenkins_logo.svg.png" width="85px;" height="85px;" alt="Jenkins"/><br /><b>Jenkins</b></a></td>
    <td align="center"><a href="#git"><img src="img/logos/1280px-Git-logo.svg.png" width="80px;" height="75px;" alt="Git"/><br /><b>Git</b></a></td>
    <td align="center"><a href="#cloudwatch"><img src="img/logos/aws-cloudwatch-logo-png-transparent.png" width="75px;" height="75px;" alt="cloudwatch"/><br /><b>cloudwatch</b></a></td>

  </tr>
  <tr>
    <td align="center"><a href="#Google app scripts"><img src="img/logos/google-apps-script-logo-BDEAA5E2DF-seeklogo.com.png" width="75px;" height="75px;" alt="coding"/><br /><b>Google App script</b></a></td>
    <td align="center"><a href="#python"><img src="img/logos/1024px-Python-logo-notext.svg.png" width="80px;" height="75px;" alt="Python"/><br /><b>Python</b></a></td>
    <td align="center"><a href="#kubernetes"><img src="img/logos/1200px-Kubernetes_logo_without_workmark.svg.png" width="75px;" height="75px;" alt="kubernetes"/><br /><b>Kubernetes</b></a></td>
  </tr>
  <tr>
    <td align="center"><a href="#docker"><img src="img/logos/docker_logo.png" width="80x;" height="75px;" alt="Docker"/><br /><b>Docker</b></a></td>
    <td align="center"><a href="#flask"><img src="img/logos/flask-logo.png" width="75x;" height="75px;" alt="Flask"/><br /><b>Flask</b></a></td>
    <td align="center"><a href="#gatling"><img src="img/logos/gatling.png" width="70px;" height="75px;" alt="Gatling"/><br /><b>Gatling</b></a></td>
  </tr>

# Jenkins 
Jenkins is an open source automation server which enables developers around the world to reliably build, test, and deploy their software.

### Step 1: Jenkins Installation


1. Install java dependencies:

```shell
sudo apt install openjdk-11-jdk -y
```

2. Install jenkins:

```shell
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

3. Check the status of Installation:
```shell
sudo systemctl status jenkins
```
### Jenkin Plugin Prerequisites 
- Docker 
-...


## Job to pull from Dev Branch 

### Step 1: Set up webhook on GitHub

1. In your GitHub repository go to `Settings` and select `Webhooks` 

2. Select ` Add webhook` 

3. For the Payload URL you must put `http://<jenkins-url>/github-webhook/`

4. For Content Type select application/json. 

### Step 2: Build a Job to Pull from Dev branch  

When there is a push to the dev branch, Jenkins will 

- Discard old builds 
    - Max # of build : 3
- Github project
    - Insert GitHub HTTPS repo link
- Source Code Management
    - Insert SSH Repo link
    - Provide the SSH private Key which should be connected to your Github repo in Credentials
    - Branches to build : */dev
- Build Triggers
    - Select GitHub hook trigger for GITScm polling
- Build 
    - Execute shell: test code 
- Post-build Actions
    - Select the Projects to build 
    - Trigger only if build is stable

### Step 2.5: Build a Job to merge into main
- Discard old builds 
    - Max # of build : 3
- Git
    - Repository URL
    - Credentials
    - Branches Specifer: */dev
    - Branches Specifer: */main
- Additional Behaviours: Merge before build
    - Name of repo: origin
    - Branch to merge to main
- Post-build Actions
    - Git Publisher
        - Push Only if Build Successeds
        - Merge Results

### Step 3: Build an Docker Image 
This job will pull the code from the main branch and build a docker image. The build will then push the images to the DockerHub repo. 



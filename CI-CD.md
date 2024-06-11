# CI/DC Bootcamp

### 1. [CI With Jenkins](#1.-ci-with-jenkins)
### 1.1 [Why need CI & CD?](#1.1-Why-need-CI&CD?)
### 1.2 [CI&CD Concept](#1.1-Why-need-CI&CD?)
### 1.3 [Why Jenkins?](#1.3-Why-Jenkis?)
### 2. [Practice](#2.-Practice)

## 1. CI With Jenkins
### 1.1 Why need CI&CD?
Applications are highly interconnected and any change on a module, whether in the infrastructure, the backend or the frontend, can affect the performance of the application and lead to integration issues on every change. Thus, it represents a challenge in the sofware development lifecycle because new features and bug fixes can take a long time to be delivered.

CI&CD is a methodology aimed to solve these callenges by introducing automated testing and automated deployment for every application change, thus reducing the integration issues and the time to market for new features to reach application's customers.

### 1.2 CI&CD Concept

Continous Integration (CI) refers to the development phase of the application. The Developers use CVS systems like git to version control the produced software. After each commit the automated testing takes place to check if the software is working properly and more importantly if the application as a whole is also working as expected. Figure 1 shows an example of CI.

CI rebuilds the whole application on every change introduced. This is particularly useful because it prevents changes that don't pass the automated testing to be deployed in production, therefore integration issues are quickly discovered and fixed inmediatly. 

<p align="center">
  <img src="https://github.com/danny-zh/epam_intro_cloud_devops/assets/134888524/1d918fdf-4d0b-4e4f-87bd-83825254265b"
         width="600" height="300"/>
  <br/>
  <em>Figure 1. CI&CD Workflow</em>
</p>

CI must have the following:

- VCS: Normally git repositories to version control the code of the application are used. The builds are taken usually from the last commit. 
- Build Automation: Refers to the ability to build on every change the new code that was added in the VCS. Scripts are usually used because they are reliable and less prone to error as compared to humans.
- Automated Testing: Refers to the ability to test the introduced changes in the module. Scripts and programs are used to achieve this goal.
- Often Commits: Commit each 2 or 3 hours, if an error or bug is discovered in the testing phase it can narrow down the search to look where the problem is located.
- Build on every change: As soon as a new change is introduced in the code, the CI must be automatically started.

Continously Delivery refers to the deployment phase of the application. The goal is to have a fully functioning system after every change. The automation of the deployment is also introduced in this phase.

Overall the benefits of CI are:

- Defect early discovery: Errors are discovered and fixed more quickly
- Automatic application deployment: Automated testing and deployment guarantees that the code is working properly and that the changes are automatically deliver to production
- Transparent health monitor: Monitor that shows the status of a particular build. The build can take some time to complete, but it is important to monitor the result of the build process.
- Test pipeline customization: Builds can be chained
- Built-in parallel execution: Distribute building jobs to underlying hosts.

### 1.3 why Jenkins?
It is by far the most popular and used CI&CD tool.

- Free and open source
- Supported in various platforms (windows, linux, docker, cloud)
- Big support community
- Plugins for extended functionality


<p align="center">
  <img src="https://github.com/danny-zh/epam_intro_cloud_devops/assets/134888524/c996c1b3-463b-4909-977a-b965413eaf1d"
          width="600" height="300"/>
  <br/>
  <em>Figure 2. CI&CD Tools</em>
</p>

### 1.3 Install Jenkins

Must:
- JDK with compatible version must be installed (JKD 7, 10 , 21)
- In windows to start or stop the process you must do **win+r**, then type **services.msc** and look for jenkis service
- Follow installation guide from https://www.jenkins.io

### 1.4 Build Triggers

Build triggers allows to automatically start a build job when a condition or criteria is met. Most common build triggers are:

1. On-demand run: Manual
2. Other job finish: Start when anoter job finishes successfully
3. Cron schedule: Set an calendar based schedule
4. SCM check: Source code management, for tracking commits in CVS. It works based a cron expression. For load balance the variable **H** is preferred to be used to avoid several jobs looking for changes in the CVS at the same time.

### 1.5 Cron Syntax

Cron is a built-in utility in jenkins that allows execution of jobs given a certain schedule. Figure 3 shows the sintax of cron

[Crontab](https://crontab.guru/) is an excellent web based app to check for the cron expression for a particular need.

<p align="center">
  <img src="https://github.com/danny-zh/epam_intro_cloud_devops/assets/134888524/00e37395-09f7-4c74-bfc4-ec351157fc05"
          width="600" height="300"/>
  <br/>
  <em>Figure 3. Cron Syntax</em>
</p>

## 2. Practice


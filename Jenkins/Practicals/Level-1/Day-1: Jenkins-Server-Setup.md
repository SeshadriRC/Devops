The DevOps team at xFusionCorp Industries is initiating the setup of CI/CD pipelines and has decided to utilize Jenkins as their server. Execute the task according to the provided requirements:



1. Install Jenkins on the jenkins server using the yum utility only, and start its service.

If you face a timeout issue while starting the Jenkins service, refer to this.
2. Jenkin's admin user name should be theadmin, password should be Adm!n321, full name should be Ravi and email should be ravi@jenkins.stratos.xfusioncorp.com.


Note:

1. To access the jenkins server, connect from the jump host using the root user with the password S3curePass.

2. After Jenkins server installation, click the Jenkins button on the top bar to access the Jenkins UI and follow on-screen instructions to create an admin user.


# Solution


- Login to the Jenkins server
- Navigate to Jenkins website and install it [Website](https://www.jenkins.io/doc/book/installing/linux/#red-hat-stable)
- Install wget utility if not installed
  
  ```
  yum upgrade
  yum install wget
  wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  yum install fontconfig java-21-openjdk
  yum install jenkins
  ```
  - Start the jenkins service [Link](https://www.jenkins.io/doc/book/installing/linux/#start-jenkins-2)
  ```
  systemctl enable jenkins
  systemctl start jenkins
  systemctl status jenkins
  ```
  - Unlock the user and configure given user [Link](https://www.jenkins.io/doc/book/installing/linux/#unlocking-jenkins)

  - Get the password
  ```
  cat /var/lib/jenkins/secrets/initialAdminPassword
  ```
  - Open the link and configure user

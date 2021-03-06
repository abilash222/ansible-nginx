Automate Nginx Installation and configuration update
=================

Install Ansible
=================

sudo amazon-linux-extras install ansible2
 
Install Jenkins
=================
* sudo yum install java-1.8.0-openjdk-devel
* curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo
* sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
* sudo yum install jenkins
* sudo systemctl start jenkins
* sudo systemctl status jenkins
* Run netstat -lntp and verify port 8080 is running
* once Jenkins is started do the necessary initial configuration as instructed.

Ansible Repository
======================
* Create a repository in Github.
* Write the necessary ansible scripts to deploy and update nginx configuration

Configure Jenkins Job
======================
* Create a Secretfile file with provided login key agnel.pem
* configure a freestyle jenkins job with SCM git and do the necessary configuration.
* In Build Triggers section select Poll SCM and schedule to run every 5 minutes(H/5 * * * *)
* In the Build Environment section select Use secret text(s) or file(s) and bind the secretfile created in previous step
* In the build section execute shell command as below
 "rm -f $WORKSPACE/files/agnel.pem
 mkdir -p $WORKSPACE/files 
 cp $LOGIN_KEY $WORKSPACE/files/
 ansible-playbook  -i inventories/hosts playbook.yaml"

Now the Jenkins will look for any changes in the repository every 5 mins and runs the ansible playbook whenever changes are detected.

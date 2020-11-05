# Jenkins Installation:
1.  Install java-1.8.0-openjdk-level:
- sudo yum install -y java-1.8.0-openjdk-devel
- sudo yum install -y wget
2. Downbload the repo:
- sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
3. Import the required key:
- sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
4. Install Jenkins
- sudo yum install -y jenkins
5. Enable Jenkins
- sudo systemctl enable jenkins
6. Start Jenkins
- sudo systemctl start jenkins
7. In a new browser tab, navigate to http://<PUBLIC_IP_ADDRESS>:8080, replacing <PUBLIC_IP_ADDRESS> with the IP address of the cloud server provided on the lab page.
8. We'll be taken to an Unlock Jenkins page telling us we need to locate the password. In the terminal, run:
- sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# Builds and Build Management: 
1. Building in Jenkins
- Create a folder named Test
    - Dashboard -> new item -> Test -> folder 
    - Inside Test Folder -> new item -> User_test -> freestyle project -> (Build) -> Execute shell -> uname -a && whoami > user_test.txt -> build now
- Create a user test job that saves it output in a file "user_test.txt" 
    - workspace -> user_test.txt
2. Building from SCM
    - configure Maven installer
        1. login to Jenkins server
        2. click Manage Jenkins
        3. click Global Tool Configuration
        4. Under Maven Installations, click Add Maven
        5. In the Maven box, enter "M3"
        6. Make sure Install automatically is clicked. 
        7. Save
    - configure the build to use Maven and Make the index file
        1. click New Item
        2. enter an item name of the "mavenproject" in the box provided.
        3. select freesytle project
        4. click OK
        5. click the source code management tab at the top of the screen.
        6. select the option for the Git repository. 
        7. copy the Git repository link https://github.com/cgpeanut/jenkinsbydoing.git and enter it into the Repository URL box.
        8. click the Build tab at the top of the screen
        9. click add a Build Step and select the invoke top-level Maven Targets option.
        10. Under Maven Version, select M3
        11. In the Goals box, enter "clean package".
        12. Click Add build step and select the Execute shell option. 
        13. In the command windows, enter "bin/makeindex".
        14. click Add post-build action and select the Archive the artifatcs option.
        15. Inside the Archive the artifacts box, click Advanced.
        16. Check the option for the Fingerprint all archieved artifacts.
        17. In the files to archive box, enter "index.jsp"
        18. click Save
        19. click Build Now
        20. Refresh window and click View link next to index.jsp. Verify the contents of the index.jsp file. 
# Distributing a Build (adding Build Node)
    - configure Maven to build project pulled from SCM - configure a slave node to builkd project instead of master node. 
        1. In slave machine, su as root  vim /etc/passwd
        2. In the last line in the file (beginning with jenkins), change /bin/false to /bin/bash to allow the jenkins user a shell login.
        3. Save
        4. Change the password for the jenkins user: passwd jenkins
        5. switch to jenkins: su jenkins
        6. change dir: cd ~
        7. Generate public/private RSA key pair: ssh-keygen
        8. login to slave server
        9. become root: sudo su
       10. create a jenkins user: useradd jenkins
       11. create passwd: passwd jenkins
       12. open the sudoers file: visudo
       13. In the Defaults section, mbeneath root, add: jenkins ALL=(ALL) NOPASSWD: ALL
       14. save and exit
       15. exit root
       16. see who you're login as: whoami 
       17. switch to jenkins: su jenkins
       18. change dir: cd ~
       19. As the jenkins user on the master server, copy the jenkins user's ssh keys to the slave server:
       20. cat ./.ssh/id_rsa

# Run the Maven Build on the Remote Agent

In a new browser tab, navigate to http://<JENKINS_MASTER_SERVER_PUBLIC_IP>:8080.

Log in to Jenkins using the following credentials:

Click Manage Jenkins in the left-hand menu.

Click Manage Nodes and Clouds.

Click New Node.

Give it a name of slave1.

Select Permanent Agent.

Click OK.

For Remote root directory, enter /home/jenkins.

For Labels, enter slave1.

For Host, enter the slave server's public IP address.

Next to Credentials, click Add > Jenkins.

Set the following values:

Kind: SSH Username with private key
Username: jenkins
Private Key: Enter directly
Copy the entire RSA key in the terminal (from dashes to dashes) and paste it into the Key window
ID: jkey
Description: jenkinsuser
Click Add.

Set Credentials to jenkins (jenkinsuser).

Click Save.

In the upper-left corner, click Jenkins > New Item.

Enter an item name of mavenproject.

Select Freestyle project.

Click OK.

Set the following values:

General
Restrict where this project can be run: Check
Label Expression: slave1
Source Code Management
Git: Check
Repository URL: https://github.com/linuxacademy/content-cje-prebuild.git
Click outside the box to make sure the red text goes away.
Build
Click Add build step > Invoke top-level Maven targets.
Goals: clean package
Click Add build step > Execute shell.
Command: bin/makeindex
Post-build Actions
Click Add post-build action > Archive the artifacts.
Files to archive: index.jsp
Click Advanced....
Fingerprint all archived artifacts: Check
Leave other default boxes checked.
Click Save.

In the upper-left corner, click Jenkins > Manage Jenkins > Global Tool Configuration.

In the Maven section, click Add Maven.

Give it the name M3.

Click Save.

In the upper-left corner, click Jenkins.

Click mavenproject.

Click Configure in the left-hand menu.

In the Build section, set Maven Version to M3.

Click Save.

Click Build Now in the left-hand menu.

Once the build starts, click the dropdown icon next to #1 and select Console Output and observe its progress.
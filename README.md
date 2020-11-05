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
        6. Make syre Install automatically is clicked. 
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


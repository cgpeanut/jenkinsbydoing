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
- Create a user test job that saves it output in a file "user_test.txt"S

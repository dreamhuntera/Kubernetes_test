# Kubernetes_test
Tryout of deployment using **CI/CD pipeline**. Documenting the steps I took towards this mission.

**Summary workflow:**

    1. Developer writes code
    
    2. Pushes to GitHub → triggers Jenkins
    
    3. Jenkins pulls code, runs tests, builds Docker image
    
    4. Docker image stored (locally or in a registry like Docker Hub)
    
    5. Ansible prepares servers & deploys image
    
    6. Kubernetes runs the containers in production

Tools used:
GitHub - **version control** : store and tracks code

Jenkins-  **CI/CD automation**: builds, test and automates pipelines

Docker - **containerization**: packages the app with all dependencies

Ansible- **configuration and deployment automation** : sets up servers and runs deployments

Kubernetes - **container orchestration platform** : manages running apps in production


For trying to minimize all the costs and having in mind there was no heavy project to be deployed I used the free tier version of the Amazon Web Services in this exercise to learn how to apply CI/CD (continuous integration/ continuos deployment).

Plan. There must be created 3 instances in the AWS :
1. Jenkins server
2. Ansible server
3. Kubernetes server

Note: In this example Docker images were build on Ansible server. Next they will be pushed to the Docker Hub. Ansible server will run the playbook, that will execute the kubectl command through Minikube. This will fetch the latest image from Docker Hub.

 
To create the instances. First, I created an account with the AWS. 

1. First instance I lauched was the Jenkins server.

Accessed EC2. Next Launch instance. Followed the step by step the process. Selected a t2.micro instance type, being free tier and sufficient for learning purposes. Added an 8080 port with a TCP protocol so that it can be used by Jenkins.

Created a keypair and saved it locally. Launched instance.

After waiting for the instance to be initiallized.

Alongside creating these, there were also installed all the necesary dependencies. Pre-requisites: Git, AWS account, Linux, Jenkins, Docker, DockerHub Account, Ansible, Kubernetes (deployment and services)

Next connect securely with the computer at the IP address found under details public IPv4 address in AWS server instance, log in as the user 'ec2-user', using specific private key file for authentication, the keypair created while lauching the AWS instance and saved into the local machine under the extension .pem 

Command: ssh -i /path/to/your-key.pem ec2-user@your-instance-public-ip

Installed on Jenkins instance: Jenkins
    Created a Jenkins account. Installed - SSH Agent - from the list of plug-ins offered by Jenkins: section  Manage Jenkins - Plugins- Available plugins- Restart Jenkins.

2. Second instance: Ansible server.

From AWS, Launch instance. Followed step by step the procedure. Instance type: t2.micro. Created keypair: saved it locally. 
Launched instance.

- Connect to instance from terminal. Namely used this command : ssh -i /Users/dreamhuntera/Downloads/AWS-keypair/ansible-key.pem ec2-user@35.176.40.130  in the terminal (machine: mac OS) 
- create ansible.sh script in terminal. Run command: nano ansible.sh
- content of the file:

sudo apt-add-repository ppa:ansible/ansible -y
sudo apt update -y
sudo apt install ansible -y
# Update package lists
sudo dnf update -y

# Install pip (Python package manager)
sudo dnf install python3-pip -y
 
# Install Ansible using pip
pip3 install ansible --user
~                             
- exit and save: press ESC; type :wq ; press Enter
- execute file. Command: sh ansible.sh

3. Third instance: Kubernetes server
   
 From AWS, Launch instance. Followed step by step the procedure. Instance type: t2.micro. 
 
 Note: Usually in need of a bigger space, in the example I am following they used a t2.medium, but I considered that for learning purposes, being free tier, this would be enough. Will see if it will work going on with the steps. If not, I will come back and create another instance.

Created keypair: saved it locally. 
Launched instance.  

Created in VS Code a simple **Docker file**. Short overview of the content:

- starts Docker image based on latest version of CentOS (popular Linux distribution). Foundation (OS) for the environment in building.
- add label to the image. Shows who maintains it.
- Install Apache web server (httpd), zip and unzip tools using CentOS package manager (yum).
  -y  means = yes to all prompts during installation
  \  lets you continue command across multiple lines

- Downloads a ZIP file (free website template) from the internet and directs it to a directory container indicated by pathway  "/var/www/html/"
- Change of the working directory inside the container. All following commands (such as RUN, COPY) will be executed from this location
- RUN unzip  extracts content of the "photogenic.zip" file in the current directory
- copies all files from the photogenic subfolder into the current directory
    -r : recursive(includes subfolders)
    -v: verbose(shows what is happening)
    -f : forcce overwrite if needed

- RUN rm -rf  : deletes original ZIP file and the extracted folder since the files are already moved. Purpose: cleap up and reduce image size
- Runs Apache in the foreground once the container starts. Purpose: Keep the process alive
- EXPOSE 80 : tells Docker the container will listen on port 80. Defaut port for HTTP web servers



Created a public GitHub repository **Kubernetes_test**.
**push** the Docker File in the repository

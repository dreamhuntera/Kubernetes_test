# Kubernetes_test
Tryout of deployment using CI/CD pipeline. Documenting the steps I took towards this mission.

For trying to minimize all the costs and having in mind there was no heavy project to be deployed I used the free tier version of the Amazon Web Services in this exercise to learn how to apply CI/CD (continuous integration/ continuos deployment).
I created an account with the AWS to create Instances. 

First of all, there must be created 3 instances in the AWS :
1. Jenkins server
2. Ansible server
3. Kubernetes server

Alongside creating these, there were also installed all the necesary dependencies.


First instance I lauched was the Jenkins server.
Installed SSH Agent from the list of plug-ins offered by AWS.
Accessed EC2. Next Lauch instance. Followed the stept of the process. Selected a t2.micro instance type, being free tier and for learning purposes. Added an 8080 port with a TCP protocol so that it can be used by Jenkins.
Created a keypair and saved it locally. Launched instance.
After waiting for the instance to be initiallized.

Next connect securely with the computer at the IP address found under details public IPv4 address in AWS server instance, log in as the user 'ec2-user', using specific private key file for authentication, the keypair created while lauching the AWS instance and saved into the local machine under the extension .pem 

Namely used this command : ssh -i /Users/dreamhuntera/Downloads/AWS-keypair/ansible-key.pem ec2-user@35.176.40.130  in the terminal (machine: mac OS) 



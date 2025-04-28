# LibraryManagementSystem(AWS-GitlabCICD-Github Webhook) Setup Guide 
This project is a web-based library management system developed using the Flask framework. It is designed to be containerized with Docker and automated through GitLab CI/CD pipelines.
Additionally, the project is connected to GitLab via GitHub webhooks. Whenever new changes are pushed to the GitHub repository, the webhook triggers GitLab, which then executes the .gitlab-ci.yml pipeline.

As part of the deployment process:

The updated project is automatically transferred to an AWS EC2 instance.
A new Docker image is built on the server.
An updated Docker container is created and deployed automatically, ensuring the application is always up-to-date.

* Backend: Python (Flask)
* Frontend: HTML5, CSS3, JavaScript
* Database: SQLite
* Containerization: Docker
* CI/CD: GitLab CI (triggered via GitHub Webhooks)
* Deployment: AWS EC2
* Version Control: Git (GitHub)
  
* Default admin email:admin@kutuphane.com password:admin123
* Default user email:kullanici@kutuphane.com password:test123
* Default admin register code : ADMIN123

![white_background](https://github.com/user-attachments/assets/c367799f-c87e-4fe0-ad3d-23e6d6810b0f)

* Create security group for ec2 instance

   ```shell
   aws ec2 create-security-group --group-name my-sg --description "My security group"
   ```
* Allow Port 80 (HTTP)
  
   ```shell
   aws ec2 authorize-security-group-ingress \
    --group-id $SECURITY_GROUP_ID \
    --protocol tcp --port 80 --cidr 0.0.0.0/0
   ```
* Allow Port 5000 (FLASK)
  
   ```shell
   aws ec2 authorize-security-group-ingress \
    --group-id $SECURITY_GROUP_ID \
    --protocol tcp --port 5000 --cidr 0.0.0.0/0
   ```
* Allow Port 22 (SSH)
  
   ```shell
   aws ec2 authorize-security-group-ingress \
    --group-id $SECURITY_GROUP_ID \
    --protocol tcp --port 22 --cidr $(curl -s https://checkip.amazonaws.com)/32
   ```
* Create Keypair
  
   ```shell
  aws ec2 create-key-pair --key-name my-keypair --query 'KeyMaterial' --output text > my-keypair.pem
            
* Create EC2 Instance
   ```shell
   aws ec2 run-instances \
    --image-id ami-0090963cc60d485c3 \
    --count 1 \
    --instance-type t2.micro \
    --key-name MyKeyPair \
    --security-group-ids $SECURITY_GROUP_ID \
   ```
* Connect EC2 Instance
   ```shell
   ssh -i yourkeypair.pem ec2-user@ec2publicip
   ```
![Screenshot 2025-03-08 130533](https://github.com/user-attachments/assets/4289fc03-cfb4-41ee-b860-5dd35aeaa3c1)

* Creating github webhook for gitlab 
   ```shell
   https://gitlab.com/api/v4/projects/(gitlab project id)/trigger/pipeline?token=(gitlab runner token)&ref=main
   ```
* Its must be look like this after entering payload url 
![Screenshot 2025-03-11 125543](https://github.com/user-attachments/assets/eae20375-e5e8-4f31-b297-201e07fe1e6f)

* After creating webhook its push every update you made in github repo to the gitlab ,  now you can access web server
   ```shell
   publicip:5000
   ```
* In server footages

![chrome-capture-2025-4-28 (1)](https://github.com/user-attachments/assets/1113091d-0275-4c40-ba4f-4c54a350e035)

![chrome-capture-2025-4-29](https://github.com/user-attachments/assets/95d46732-097d-4d69-8d79-0b2685e50cfe)

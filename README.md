# AWS

## AWS Introduction 

- AWS is stand for Amazon Web Services, which is a collection of many Services 

## IAM - Manages Users, Roles and Permissions 

- IAM (Identity and Access Managment) allow me to manage who has access to my AWS Infrastructure who can configure stuff . I can do that by creating User or User Group and then assigning them certain Permission for different Services

- By default I have Root User . Root User has unlimited privileges .

- Once I have account I need to create Admin User with less privileges to work with AWS . Basically I still want to administer the account so I need administrative access, however I should reserve Root User for Special Administator Task like changing account setting, billing infomation, etc ...

- Admin User also has Permission to create Users and Role for those Users

- In addition to Human user I also have System User . For example I have an Application like Jenkins that automatically deploy Docker container on EC2 Instances . These System User also need access to AWS .

### 3 Type of IAM Resources

- User Groups: If I have 10 Users all hav the same Privileges or 10 build Tools like Jenkins all have the same Privileges then I can create a Groups of Users to group all those User that have the same Permission

- Users: are Human User or System User that I want to give access to my AWS account  

- Roles: Granting AWS Services access to other AWS Services

### IAM User (Human or System) and IAM Roles

<img width="600" alt="Screenshot 2025-03-28 at 10 32 28" src="https://github.com/user-attachments/assets/afc1786a-70c9-436f-939b-ca391ddf44be" />

- Users are assigned to Users who are using my AWS account and create Services, configuring them .

 - Use case for this is let's say I am a Devops Team and I am configuring EC2 Instances, configuring them on AWS account . I can delegate some of that Configuration, maybe Provisioning to other AWS Services so the other Services can also create EC2 Instance . Basically now another AWS Service will act as a User and that mean that Service must has the same Permission as I have to perform the same task . However in AWS I can not assign Permission directly to Services. Instead I assign Service a Role and then I assign a Policy to that Role

 - So now Let's say I am a Devops team member and I have permission to create EC2 Instances . It mean I am as a User get created in AWS and Permission, Policy for working with EC2 Instances . Now I want to use AWS Service like EKS, ECS that will create EC2 Instances , for this I will create a Role and give that Role a Policy to work with EC2 Service and then attach that Role to EKS or ECS

 - !!! Note : I have a Role for each Service bcs a Role will have permission, or policy attached to it . And that Policy is specific to a Service for example ECS will have it's own Role, EKS will have it's own Role .  

- For those example above . Whenever I create a Role there is alway has 2 steps

 - First : Assign that Role to AWS Service (EKS, ECS, etc....)

 - Second : Attch Policy to that Role 

### Create IAM User

- Go to IAM -> Add User -> Give console Access -> Set Permission 

- I can give User 2 types of access

 - Access through a Console in the UI (Hit the Checkbox) . Access using Username and Password 

 - Programatic access which I can assign to the IAM account in AWS after I have created the user account . This is when I want to execute different task from command line . So that mean I install aws CLI and execute aws command from there or this could also be for a Services like Jenkins that need programatic access .

 - Programatic access using Access key and Secrect Access key

#### Set Permission 

- Best practice is to create a Group for Users and then assign Permission to that Group (But if for Admin user I will create only one User)

- Then I can Choose Permission for that User or Group . I have granular permission policies in AWS, so I can decide for each single Service what type of access I want to grant to User . This is a good bcs It support the Best Pratice giving the User the Least Privilege they need

- If there is Admin user I will give Addministrator access 


#### Programatic Access 

- Under Security Credentials -> Go to Access Key -> Create Access Key -> Choose CLI


## Region and Availibility Zone 

- Cloud Provider have physical center where all the Server Running .

- Data Center all over the world

- Most of Platform (AWS, Google Cloud, Micorsoft) have datacenter distributed all over the world in something called Regions .

- Regions are Physical Localtion where data center are Clustered (London, Sanfrancisco, Singapore ....)

- Whenever I create Virtual Machine (EC2) in AWS or other platform I have to choose Region . Which Region my Virtual Machine should be coming from .

- AWS also offer backup, reliability, replication for my Infrastructure . Let's say I have some complex project and I spend a lot of time and invested a lot of time to set up a complex Infrastructure on AWS . I have configured all this stuff and this is all running in 1 Region, If something happen to that Data Center I may lose my data . In order to ensure against that in these Region AWS has multiple Data Center

- Availability Zone = 1 or more discrete Data Center 

## Virtual Private Cloud (VPC)

**What is VPC**

- Virtual Private CLoud is my own Isolated Network In the Cloud (Isoliated my Network from others. Bcs others might use the name Network from that Data center)

- Whenever I create account I will have Default VPC in each Region

- VPC span all Availability Zone (Subnet) in that Regions

!!! NOTE : My Resources have to be alway running in the VPC

- In the bigger Company they could use multiple VPC in multiple different Region

- VPC is like a Virtual Representation of Physical Network Infrastructure: Server set up , Network configuration moved into the Cloud

**Subnet 1**

----Subnet----

- Subnet is a Range IP Address in my VPC

- It is a Private Network inside Network

- VPC span the whole Region . Subnet span in each Availability Zone so I have Subnet for each Availability Zone .

----Private and Public Subnet----

- Base on my Configuration I can have Private and Public subnet 

**Subnet part 2**

- In VPC I have a Range of Private IP Address (Internal IP Address) . That Range defined by default and I can change its Range

- When I create a new resources such as an EC2 instances, an IP Address is assigned within this subnet's IP range

- This a allow communication inside the VPC

- This Private IP Address only for communication inside the VPC . If I want accessible from the outside I need to assign the Public IP Address

- Public IP Address Configure in VPC Service . For allowing Internet Connectivity with my VPC I use the Internet Gateway

**Internet Gateway (Router)**

- Allow me to connect the VPC or its Subnet to the Internet(Outside Network)

**Controll Access - Security - Firewall Rule**

- I need to control what traffic Enter my VPC

- I need to control what traffic access to my Individual Instances

- And I need to control what traffic goes out

----Network ACL (Access Control Lists)----

- Control access on Subnet Level

----Security Group----

- Control Traffic on Instances Level

## CIDR Block | Classes Inter-Domain Routing 

- CIDR Block is a Range of IP Address . Normal IP address look like this `172.31.32.0` . With CIDR Block is like this `172.31.32.0/20`, a `/20` is basically would give that a Range instead of just have 1 IP Address

#### How to choose a CIDR Block ?

- IP Address represented as a binary digits and there is exact 32 of those digits . 

- Depend on how many IP Addresses I want in my VPC, I can decide how to set a Range .

- Once I have overall Range for VPC then I can give Subnet inside the VPC subCDIR blocks

- To caculate CIDR Blocks :

  - IP Calculator: https://mxtoolbox.com/subnetcalculator.aspx
    
  - IP Calculator with binary values: http://jodies.de/ipcalc
    
  - Calculate sub-CIDR blocks: http://www.davidc.net/sites/default/subnets/subnets.html

## EC2 | Virtual Cloud Server 

- EC2 Stand for Elastic Compute Cloud . This is a Server that running in one of Amazon's Data Center . EC2 is categorized under Compute Category bcs It is a Service that provide Compute Capacity

#### Deploy Web App on EC2 Intances 

- Step 1: Create EC2 Instance

  - Click Launch Instance
 
  - Give it a Name
 
    - Additional to Name : Meta data as key-value pair . Infomation about what type of server this is . Any key-value pair that I want . Which is helpful when I have alot of Instances for filtering them out or in billing to see whar resources cost me how much and so on ...
   
    - I also can add Tag to all Components on AWS . It help me filter different type of Resources with the same text .
   
  - Select OS Image and Application
 
    - I have a list of Type for each Instances . Basically Each Instance is configurable with different Resources Size (CPU, RAM, Storage, Network Performence)
   
  - Key pair (Login)
 
    - In order to SSH to Instances I have to choose key pair for my Instances . I can create key pair on AWS and then I can dowload Private Key locallly and the Public key automatically store by AWS
   
    - I can create new one per Instance
   
    - If on Mac or Linux choose .pem . If on Window choose .ppk
   
  - Network Setting :
 
    - I have VPC . Default VPC allocated by AWS . If I have multiple VPC I can choose which VPC I can start my Instance in
   
    - I also have Subnet . Each Region have 1 to 5 Availability Zone = Subnet depend on that Region .
    
    - I can choose any Subnet in that VPC to start and this make sense bcs if I configured my Subnet differently . For example I have configure Firewall rule each Subnet differently . Maybe I don't allow Traffic to one Subnet at all make it Private Subnet ... etc
   
    - I also can Assign Public IP address . If it enable , After Instance created it will get Public IP address also Private IP address .
   
    - I also can Configuring Security Group (Firewall Rule) . Each Subnet can have it's own Firewall rules  where I decide what traffic I allow on which Port (Subnet level = NACL Network Access Control List) . Or the Alternative I can define the Fire Rule on Instances Level (Security Group)
   
    - To be more Secure . I only allow my IP address (or a set of IP Address) to SSH to the Instance 

  - Configure Storage
 
- Step 2 : Connect to a Server via SSH

  - Save the `.pem` file to more secure location . Move to .ssh folder `mv .pem ~/.ssh/` . And then I change Permission of that file to make a Permission Stricter `chmod 400 .pem`.
 
  - Get Public IP Address of my Instance so I can connect to it . And I need to SSH to a Instance as EC2 user not a root User (Which is as a default): `ssh -i <pem-file> ec2-user@<public-ip-adress>`

- Step 3 : Install Docker on EC2

  - First I need to check my Package Manager tool for any update . Always I need to run Update for my Package Manager Tools `sudo yum update`

  - Install Docker : `sudo yum install docker`
 
  - Start Docker Daemon : `sudo service docker start` . I have to start Docker before pull Image , Run container etc ...
 
    - Docker Daemon is the core engine behind Docker . It's background process that run on my host system and handle heavy lifting of building, running and managing Docker Container .
   
      - Listen for Docker API (CLI or tools)
     
      - Build and Run container
     
      - Manage Image, Network and Volume
     
      - Communicate with Docker Registry like Dockerhub to pull and push Image

    - I want to able to execute Docker Command without sudo . In order to do that I need to add EC2 User (which is my EC2 Intances User) to Docker Group : `sudo usermod -aG docker $USER` . Then I need to exit and SSH again for it to effect 
   
      - $USER is stand for current User   
   
- Step 4 : Run Docker Container (Docker login, pull, run) from Private Repo

  -  Login to Docker Hub to get Image : `docker login` . Docker hub is a default so I don't need to provide Docker URL . If I have Nexus or ECR I need to provide URL of those Repo
 
  -  Locally it will create hidden file .docker/config.json . This config.json file auto generate when I do `docker login` . This will now hold Credentials or Authentication Token to a Docker Repository so I don't need to repeatedly provide username and password everytime I login 
 
  - Pull Docker Image : `docker pull <repo-name/image-name:tag>` . To check Image available : `docker images`
 
  - Run Container : `docker run -p 3000:3080 <image-name> -d` . To check Container running : `docker ps`
 
    - Port 3000 : Bind Port 3000 on EC2 Server 
   
    - Port 3080 : Is where my Application is running  

- Step 5 : Configure EC2 Firewall to access App externally from browser

  -  My Instance has a Security Group attached to it that I created in Step 1 which has Firewall Rule Configure . So now I need to open a Port 3000 so the Request coming from the Browser acutally enter my instances at port 3000
 
  -  Inbound Rule are coming into my Server
 
  -  Outbond Rule are traffic rule leaving the Server . Like I did `yum update`, `yum install docker` . This is  all traffic goes outside to the internet in order to fetch those tool from the Internet .


#### EC2 Dashboard Overview 

- In Detail : I have Instance ID , Instance State, Subnet, Private and Public IP Address , DNS name for Private and Public IP Address 

  - Private IP Address is for Components inside VPC to communicate
 
  - Public IP Address is for allow me to SSH into it and then access my Application through the Browser
 
- In Security : I have Security Group and all the Firewall Rule that applied to my Instance

- Networking : I have Public and Private IP , Subnet ID, Availability Zone

- Storage

- Status Check

- Tags : I can add new tag 

## Deploy to EC2 Server from Jenkins Pipeline CI/CD 

#### Overview

- Connect to EC2 server Instance from Jenkins Server via SSH (ssh agent)

- Execute Docker command on that Server 

#### Install SSH Agent Plugin and Create SSH Credential Type 

- Go to Manage Jenkins -> Plugin -> SSH Agent

- Create SSH Credential

  - I need to make .pem file from local also available in Jenkin
 
  - Go to Multi Branch Pipeline -> Credentials -> Select SSH Username with Private Key -> Get the content from .pem file and add it into Private in Jenkin
 
#### Jenkinfile Syntax for a Plugin 

- There is a way in Jenkin of seeing pipeline syntax or Jenkinfile syntax : Go to Multi branch Pipeline -> Select Pipeline Syntax -> In the Sample Step I can choose diffent steps that I want to configure -> This case I choose ssh agent -> then I will have a list of credentials available for this project

#### Jenkinfile | Connect to EC2 and run Docker command 

 ```
 stage("deploy") {
    steps {
        script {
            echo 'deploying docker image to EC2...'

            dockerCMD = 'docker run -p 3000:3080 nguyenmanhtrinhrepo/demo-app:1.0'

            sshagent(['ec2-server-key']) {
              sh "ssh ec2-user@public-ip-address ${dockerCMD}"
            }
        }
    }               
}

 ```
- In deploy Step I will paste in the generated syntax from above example

  - SSH to the Instance: `sh 'ssh ec2-user@public-ip-address'` . In addition to that I need to depress the pop up bcs this is not a interactive mode: `sh 'ssh -o StrictHostKeyChecking=no ec2-user@public-ip-address'`
 
  - Passing on Docker command as a last Parameter -> Put docker run command into a variable `def dockerCMD = 'docker run -p 3000:3080 nguyenmanhtrinhrepo/demo-app:1.0'` -> And then put the var into the ssh command `sh "ssh ec2-user@public-ip-address ${dockerCMD}"` . Before this step I have to have `docker login` executed in EC2
 
#### Configure Firewall rule on EC2 

- I need to give Jenkins Server's IP address Permission to connect to EC2 Instances

- Go to Security -> Add Ip Address of Jenkins to Port 22 .

- Also Open the Port 3000 . This is the Port where Application will be accessible at so I can access it from the Browser

#### Executing complete Pipeline 

- I have a Jenkinsfile that

  - Using a Share Library : This Share Library contain all of the function for bulding the Jar of the Maven Application and then building the Docker Image
 
  ```
  library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
    remote: 'https://gitlab.com/twn-devops-bootcamp/latest/09-aws/jenkins-shared-library.git',
    credentialsID: 'gitlab-credentials'
    ]
  )
  ```

  - In the Pipeline I define an ENV with Image Name :

  ```
  environment {
    IMAGE_NAME = 'nguyenmanhtrinh/demo-app:java-maven-1.0'
  }
  ```

  - In Build Jar Stage I have `buildJar()` function from Share Lib to build the Jar file
 
  - In Build Image Stage I have Build image, dockerLogin, dockerPush function to build an Image
 
  - In the deploy Stage I have used ssh agent to ssh to EC2 . And Run the Docker Container from there
 
#### Notes 

- This approach using the ssh agent plugin is applicable for all the Servers (EC2, Digital, Linode ...)

- This just a basic simple step to run Docker container from Jenkins : For Smaller Project

- More complex set up to deploy Container I would use Kubernetes


#### Using docker-compse 

- Maybe I have 5 containers for my Applications and all of this define in Docker-compose file . In this case I could also use SSH-Agent, What I could do is basically from Git Repo that connected to Pipeline Job just take docker-compose file copy that to the Server once I ssh into it and then execute docker-compose command on the server

- Project Setup in my Repo I will have Docker Compose yaml file that define all the container need to run for the Application then I can start container with command `docker-compose -f docker-compose.yaml up`

- Step 1 : Install docker compose on EC2

  - Docker-compose is not in yum package . I will use `curl` command that I use which is gonna download the latest version of docker compose to the local file system : `sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose` . And then I make it executable : `sudo chmod +x /usr/local/bin/docker-compose` 

- Step 2 : Create Docker compose Yaml file for my Application

  ```
  version: '3.8'
  services:
     java-maven-app:
        image: ${IMAGE}
        ports:
          - 8080:8080
      postgres:
        image: postgres:15
        ports:
          - 5432:5432
        environment:
          - POSTGRES_PASSWORD=my-pwd
  ```

- Step 3 : Adjust Jenkinfile to execute docker-compose command on EC2 Instance

  - I need to have docker-compose file in EC2 Instance . I will copy docker-compose file from Git Repo to EC2 Instance
 
  - In sshAgent Block : `sh "scp docker-compose.yaml ec2-user@<public-ip-address>:/home/ec2-user"`. This is will execute on Jenkin and Jenkin before running all these Stages will check out the Repository so Jenkin Server has access to the file in the Repository   

    - ec2-user@<public-ip-address> : This is the remote server that docker-compose.yaml get copied to
   
    - :/home/ec2-user : This is where the docker-compse.yaml get copied to (inside the EC2)

  - I will set docker-compose command in a variable : `def dockerComposeCMD = docker-compose -f docker-compose.yaml up --detach` . Then I will set it in the sshAgent where the ssh connect to EC2 : `"ssh ec2-user@public-ip-address ${dockerComposeCMD}"`


  - The whole set up look like this  

  <img width="600" alt="Screenshot 2025-03-29 at 11 24 23" src="https://github.com/user-attachments/assets/b216cdc5-304b-4a8a-8407-d71a9cae6a08" />


#### Extract to Shell Script 

- What if I want to execute multiple command once I connect to EC2 Server . What if I need to set some variable before execute docker-compose command or what if I have other command to execute before

- I Can execute multiple Command by grouping them into a shell script and execute the shell script from Jenkin

- I will create shell file `server-cmds.sh`

 ```
 #!/usr/bin/env bash

 docker-compose -f docker-compose.yaml up --detach
 echo "Success"
 ```

- In Jenkinfile I will put execute shell command into Variable : `def shellCMD = 'bash ./server-cmds.sh'`

- I will copy the script file to EC2 : Inside sshAgent block : `sh "scp server-cmds.sh ec2-user@<public-ip-address>:/home/ec2-user"` and then run it on EC2 : `"ssh ec2-user@public-ip-address ${shellCMD}"`

- The whole code look like this :

<img width="600" alt="Screenshot 2025-03-29 at 11 42 18" src="https://github.com/user-attachments/assets/ac0d55d4-9a54-4425-bc6d-67b5cec16380" />

#### Replace Docker Image with newly built Version

- In Docker-compose the Image is hardcode . However when I produce Application I alway produce a new Version

- Suppose I am bulding another version of Image and I need to pass that Information or the Image Name to the Docker-Compose and replace the whole Image name in Docker Compose . In Docker Compose instead of hardcode image I will set it as a Variable : `${IMAGE_NAME}` . This Docker-compose file is actually called by server command shell script . In the Shell Script file before I execute Docker-compose CMD I will export the Image variable as a ENV `export IMAGE=xxx` and set it to a value that represent the Image name in Jenkinfile.

- So how do the Image variable in shell script file get Image Name value in Jenkinfile ?

  - I will pass the value to shell script file as a Parameter. I can set a parameter after Shell Script cmd and I can pass multiple Parameter to a shell script by just listing them and read those Parameter . In this case the Parameter I want to set is IMAGE variable . In Jenkinsfile I will pass value like this : `def shellCMD = "bash ./server-cmds.sh ${IMAGE_IMAGE}"` and in Shell script file I set : `export IMAGE=$1` (1 for the first parameter) .
 
----Wrap up----

- I have the Image as a ENV in Jenkinfile and passing on to Shell Script via Parameter . And a Shell Script read that Parameter via $1 and set the variable as Image and export ENV on EC2 . Now this ENV will be set on EC2 Server bcs this whole shell script get execute on the EC2 remote Server And then execute the docker-compse cmd with the ENV set in EC2

- This is what Jenkinfile look like :

 <img width="600" alt="Screenshot 2025-03-29 at 12 11 31" src="https://github.com/user-attachments/assets/eaf41bbc-f3e1-48cc-9245-ea3093c05812" />

- This is what Docker-compose Look like :

<img width="600" alt="Screenshot 2025-03-29 at 12 12 12" src="https://github.com/user-attachments/assets/2502f2ca-b282-441f-9cae-9cbd40f1460b" />

- This is what shell script look like :

<img width="600" alt="Screenshot 2025-03-29 at 12 13 17" src="https://github.com/user-attachments/assets/3a630417-c650-495f-9f75-a3802298b668" />


#### Set Image Name dynamically in Jenkinfile 

- Add the Increment Version Stage

- I will call this mvn command : `sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'` . That increment the version in Pom.xml

- and then I will read that version from pom.xml and extract that value and set it in a variable :
  
  ```
  def matcher = readFile('pom.xml') =~ <version>(.+)</version>
  def version = matcher[0][1]
  ```

- and then I will set Image name from that version as a  ENV : `env.IMAGE_NAME = "nguyenmanhtrinh/demo-app:$version-$BUILD_NUMBER"`

#### Commit version update Stage 

- Add commit version update Stage right after the deploy stage .

- This step the same in Jenkins module 

```
stage('commit version update'){
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]){
                sh 'git remote set-url origin https://$USER:$PASS@github.com/nguyenmanhtrinh/java-maven-app.git'
                sh 'git add .'
                sh 'git commit -m "ci: version bump"'
                sh 'git push origin HEAD:jenkins-jobs'
            }
        }
    }
}
```

#### This is how real project are built 

<img width="600" alt="Screenshot 2025-03-29 at 12 55 36" src="https://github.com/user-attachments/assets/c1372ac6-92a6-42b4-818a-b5d8cea6790e" />

----Summary----

- First Increment the Version which give me a new (or next) version Docker Image -> Then Build Application Jar using that Version -> Then I will use Jar file the build Docker Image with the Image set in the Increment version stage and push that Image to Repository -> And then will deploy Docker-Compose file the new Image version set dynamically -> Then I connect to EC2 using sshagent, copy docker-compose to ec2 and execute docker-compose on the server -> Once success Deploy I will committing the change with the change of version increment


- !!!NOTE : If the Stage fail the rest will be skip

- For more advance use case is Docker-compose isn't enough to manage my container . I gonna need to container Orchestration tools and deploy to that Orchestration tool then will look different than just using sshagent plugin 














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




















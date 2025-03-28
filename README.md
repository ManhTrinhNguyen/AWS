# AWS



## Virtual Private Cloud (VPC)

**What is VPC**

```
 - Virtual Private CLoud is my own Isolated Network In the Cloud (Isoliated my Network from others. Bcs others might use the name Network from that Data center)

 - Whenever I create account I will have Default VPC in each Region

 - VPC span all Availability Zone (Subnet) in that Regions

 !!! NOTE : My Resources have to be alway running in the VPC

 - In the bigger Company they could use multiple VPC in multiple different Region

 - VPC is like a Virtual Representation of Physical Network Infrastructure: Server set up , Network configuration moved into the Cloud
```

**Subnet 1**

```
 ----Subnet----

 - Subnet is a Range IP Address in my VPC

 - It is a Private Network inside Network

 - VPC span the whole Region . Subnet span in each Availability Zone so I have Subnet for each Availability Zone .

 ----Private and Public Subnet----

 - Base on my Configuration I can have Private and Public subnet 
```

**Subnet part 2**

```
 - In VPC I have a Range of Private IP Address (Internal IP Address) . That Range defined by default and I can change its Range

 - When I create a new resources such as an EC2 instances, an IP Address is assigned within this subnet's IP range

 - This a allow communication inside the VPC

 - This Private IP Address only for communication inside the VPC . If I want accessible from the outside I need to assign the Public IP Address

 - Public IP Address Configure in VPC Service . For allowing Internet Connectivity with my VPC I use the Internet Gateway

```

**Internet Gateway (Router)**

```
 - Allow me to connect the VPC or its Subnet to the Internet(Outside Network)
```

**Controll Access - Security - Firewall Rule**

```
 - I need to control what traffic Enter my VPC

 - I need to control what traffic access to my Individual Instances

 - And I need to control what traffic goes out

 ----Network ACL (Access Control Lists)----

 - Control access on Subnet Level

 ----Security Group----

 - Control Traffic on Instances Level
```

# TASKS
TASK-1 : Deploy hello-world-python-app using Dockerfile
---------------------------------------------------------------
- Firstly we need to clone the github repository (task-sheet) and write a docker file in that repo.
- Then we need to install docker . aftr installing docker check docker wheather it is installed or not.
- After writing the Dockerfile, create an image using command
- 
           "docker build -t python-image . " 
           
- By using docker image create docker container using commnd 
- 
             "docker container run  -dt -p 5000:5000 python-image" .
             
- Now after creating docker container check the  application is accessile over web using port number.
- Then i pushed  my image to ECR and make it available at ECR for further use.
  
---------------------------------------------------------------------------------------------------------------------------------------------------------

TASK-2: Create Dockerfile and Deploy php-app using Kubernetes
-------------------------------------------------------------------
Firstly we need to clone the github repository (task-sheet) and write a docker file in that repo.

- After writing the Dockerfile, create an image using command "docker build -t php-image . " 
-  By using docker image create docker container using commnd "docker build -d -p 80:80 php-image" .
-  Now after creating docker container check the  application is accessile over web using port number.
- Then i pushed  my image to ECR and make it available at ECR for further use.
Now i need to create eks cluster by using followig.

PRE-RQUISITES:

i) Setup kubectl

       curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
       chmod +x ./kubectl
       mv ./kubectl /usr/local/bin 

ii)Setup eksctl

      curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
      sudo mv /tmp/eksctl /usr/local/bin
      eksctl version

iii) Create an IAM Role and attache it to EC2 instance

 -  IAM user should have access to
 -  IAM
 -  EC2
 -  VPC
 -  CloudFormation
   
iv)Create your cluster and nodes

eksctl create cluster --name cluster-name  \
--region region-name \
--node-type instance-type \
--nodes-min 2 \
--nodes-max 2 \ 
--zones <AZ-1>,<AZ-2>
           
 IT will take 15-20 mins for creation of eks cluster. after creation validate the cluster 
 
- After this i have written deployment.yaml file as specified in the file according to kuberneties configurations and in this file i have pulled my php applicaion as an image which is previously pushed to ECR.
- In the service file specified load balancer and target ports.
- in the autoscaling.yml i have specified manifest file to horizontal pod auto scalling as if cpu utilization reaches 60%  max rplicas as 4 & 2 as a minimum replicas and if it reaches below 30% the minimum and maximum replicas should be 1.
- By using "kubectl get all " command we will get load balancer dns paste it in google then our application will be hosted .

To delete the EKS clsuter

eksctl delete cluster valaxy-cluster --region ap-south-1
---------------------------------------------------------------------------------------------------------------------------------------------------------

TASK-3 : create EKS cluster using terraform
--------------------------------------------
- For the creation of EKS cluster using terraform firstly we need to install terraform in the server . 
- Then we need to have resources by refering terraform regiestry we get vpc,security group and eks cluster resources.

- In the vpc.tf file, specified resources  for VPC and EKS are taken from terraform registery and modifiy these resource according to our need like  instance type as 't2 small' & * availability zone limited to 2 and specified subnets & count of node groups as 1 .
- In the eks.tf we provided every thing realted to eks cluster like iam roles .
- after creating all this  .tf files by using terraform validate command we can validate.
- Then by using terraform init,terraform plan and terraform apply with this command our cluster will be created .
- After creating this cluster we need to update kube/configuration file to get nodes .
 
--------------------------------------------------------------------------------------------------------------------------------------------------------- 

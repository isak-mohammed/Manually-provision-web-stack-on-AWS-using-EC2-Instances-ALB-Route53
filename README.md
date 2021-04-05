# Manually provisioning full stack web application using Amazon EC2, S3, ACM, Route 53 and ALB.
## Prerequisites
For this setup, you need the following:
1. An AWS account 
2. AWSCLI installed on your local machine
3. JDK8 installed on your local machine
4. Maven installed on your local machine
5. Domain name of your choice, so that we can access the application over the internet.

Note: we will be using default VPC for this setup and planning to add more stuff in coming weeks with separate VPCs. We can use centos7 ami for all the 4x instances or can use ubuntu 18 ami for just tomcat instance as it will be easy to control the start and stop of tomcat service with systemctl command without much setup.
## Steps Involved in the setup:
1. Clone the repo and cd into project-1
2. Login to aws account & choose appropriate region nearest to you.
3. Create security groups for the front-end and back-end services.
4. Create key pairs
5. Launch backend instances with user data [bash scripts] 
6. Configure route53 to map the names of backend services to ip address of the instances on which backend services are running. This allows frontend to connect to backend services without worrying about ip addresses.
7. Launch Frontend tomcat instances with user data and update application properties file with backend services names configured in route53.
8. Build Application from source code on local machine using maven and then upload artifact to s3 bucket
9. Then manually download artifacts from s3 on to tomcat ec2 instance by assigning IAM Role to tomcat Instance. Then replace the tomcat default application with our artifact.
10. Setup ALB with HTTPS [cert from ACM]
11. Map ALB endpoint to website name in DNS setting of your domain and verify the setup

## Brief Description:
The users will access our website using the URL (URL of your choice), we will make that URL to point to an ALB endpoint by adding a cname entry in dns settings of our domain name. (There are lot of domain registrars who provide the domain names of your choice at reasonable cost). Users will connect to ALB using https, and we will enforce this by only allowing https traffic in ALB security group and by loading the certificate for https encryption from ACM into ALB. ALB will route the request to our tomcat instances. We will only allow tomcats instances to accept traffic from ALB on port 8080 by enforcing inbound rules in tomcat security group.  


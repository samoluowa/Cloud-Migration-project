# Cloud-Migration-project

IN this project I will Migrate a package containing Python, Java, and Java ecom applications running from my virtual machine using Ubuntu OS to my AWS Instance, all the packers in the vVM will be automatically migrated to my AWS instance..

# Steps to accomplish this task:

-Export the package from the VM.

  -Upload the package to my S3 bucket.
  
  -Create VM import tool, to convert the image to AMI .
  
  -Use AMI to deploy packages to EC2 instance.
   

I will go into more details on each step below.

# Step 1: Exporting the package from the virtual machine running Ubuntu OS

-Using my Oracale VM Virtual Box, i installed Ubuntu and launched the terminal to run this file “ubuntu_httpd.shl”, this file will install all the necessary softwares onto my VM, these softwares being ‘OpenSSh-server’, Python3’, Git, Curl, and Apache3.

-This ubuntu_http.sh file will also start and enable SSH service, apache service,get ubuntu version and finally create an HTML page that will display Some static values, this will be the application I will deploy.

-The script I ran to complete this task was “bash Documents/ubuntu_httpd.sh”.
![complete file cmd run in VM](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/631f3225-5655-4ca1-a0f6-e75719458bd2)

-The above image shows the Script being run successfully and providing me with the Private IP which will be used to access 
the application on the browser.

![IP address in Browser on Vm](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/c09b7cab-f148-429a-b285-e56d2f3c9ead)
*The above image displays the application running successfully in the browser.*

-We can also confirm the softwares I installed in the VM by running commands in our terminal.
![confirming the installs on the terminal](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/c57975cd-1c52-45bd-ad77-06b7423eb626)

-Now that our package is confirmed to be running in the VM. I will now download the package from the VM to be deployed in my EC2 instance.

-After stopping my VM I exported the file from my ubuntu os in an OVA format to my local machine.

# Step 2: Upload the package to my S3 bucket.

-I will now upload the exported OVA file from my local machine onto my S3 bucket “file to be uploaded”.

-To upload the file to my S3 bucket I used my local windows terminal. The first script I ran was to upload this file to my S3 bucket “python .\s3_upload_multitrace.py”.

![install pip and use script to migrate trace file](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/3669a916-8d8d-403c-9bdc-ebf44dbee397)
*Script successfully ran.*

![confirming ubuntu disk in AWS after running script](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/5d875f4c-d04c-4543-b487-fee5b3fb3524)
*Above Image shows confirmation of Ubuntu_64-bit-disk1.vmdk file in AWS.*

# Step 3 : creating the VM import tool.

-In My terminal I ran the following files:-

-Role policy: I created a role to bind the policies to in AWS. I ran the following script in my terminal “aws iam create-role vmimport “file://trust-policy.json””
![role created script run successful](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/4cf1b181-3020-4e24-9be0-8ef74d1b6443)
*Script Successfully ran.*

![vmimport in aws ](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/620d7d6d-c4ad-4ab2-abad-6b94de354b2f)
*Confirming the role is now created in AWS*

-Trust policy: This will bind the policy to the role we have just created in AWS we ran the script “aws iam put-role-policy vmimport –policy name vmimport “file://role-policy.json” ”.
![script to add policy to role completed](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/876fee3c-f237-499c-9b03-0d2c54c7e72a)
*Script successfully ran in the terminal.*

![confirming policy added to rol in AWS](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/514dd300-1941-4ea3-9ef3-d1a5b7c3991a)
*policy successfully added to role in AWS*

-This policy gives the role permission to access the S3 bucket with my ubuntu package as well as all the resources inside the bucket.

# Step 4 : use AMI to deploy package to EC2 instance.

-Now for the final step we will convert the image file into an AMI and create an instance to run the application in AWS. 

-In my terminal using the import parameters.json file I ran the below script to convert the image file to an AMI. “ aws ec2 import-image -description “convert ubuntuOS onprem to AWS AMI” –disk-containers “file://import-parameters.json”
![convert ubunto onprem to aws ami script ](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/4e9d54b2-5509-42fc-b25a-a54534bfc92d)
*script successfully ran*

-This will take a couple of minutes ro run, I eventually used the task ID from the terminal to track the progress of the conversion in the terminal, using “python vm-import-export-task.py import-ami-04346381e931e38d0”

![script to confirm the AMI was created ](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/7612ad36-69d7-4d78-a3fa-fad8b1573d69)
*confirmed the task was completed in terminal*

![ami in AWS confirmed](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/7146e589-551a-4794-a0bf-abfe39256eab)
*This shows the AMI in AWS has now been created and is available.*

-Now we just have to create an Instance from the AMI we just created in AWS.

![creating inctance with AMI we just imported](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/83dc8001-16c6-4845-9b2a-a031b5347a72)
*Launched an ubuntu instance in AWS from the AMI I just created.*

![instance running show public ib](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/e8247550-c5e9-4372-aabe-1f780f69ad8a)
*Ubuntu instance Successfully created and running in AWS*

-Using the Public IP address I am able to access the application in my browser

![app working in AWS confirmed](https://github.com/samoluowa/Cloud-Migration-project/assets/71151040/32a94bec-238a-435a-a4e5-eb93b161d232)
*Application successfully running on browser.*

With this I have successfully migrated the ubuntu packages from my VM in Virtual box to my AWS EC2 instance.


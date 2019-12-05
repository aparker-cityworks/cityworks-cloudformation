# cityworks-cloudformation
A CloudFormation template that creates an infrastructure in which to deploy Cityworks.

[![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=Cityworks&templateURL=https://s3-us-west-2.amazonaws.com/cw-s3/cityworks.yaml)

## New Site Setup
First create an AWS account and login. Then click the Launch Stack button above. This will deploy a complete infrastructure that will support a Cityworks installation. It will create the network (VPC, Subnets etc), database server (RDS SQL server), and Cityworks app server (Windows Server w/IIS, .NET and Crystal Reports runtime).

You can see the progress of the deployment by going to the CloudFormation section in the AWS Console. Once the status says CREATE_COMPLETE, the infrastructure has been deployed.
[[img/cloudformation.png]]

As part of the deployment process, an Automation document will be run to configure the Cityworks server by automatically installing the Cityworks prerequisites and creating an initial database on the RDS server. Go to the Systems Manager section in the AWS Console and click the Automation section. You can view the progress of the configuration as it runs. Once all the steps have completed, you can login to the server and install Cityworks.
[[img/automation.png]]

Go to the CloudFormation section in the AWS Console and click on the Outputs tab of the stack we launched. This Outputs page contains a number of vital information that you will need to connect to and configure your environment.
[[img/outputs.png]]

Go to the EC2 section in the AWS Console. Right click on the Cityworks instance and click Get Windows Password. It will prompt for a private key value to decrypt the password. Copy the KeyPairPrivateKey value from the Outputs tab in CloudFormation, paste it in the box and click Decrypt Password. You will then see a Public DNS name and Admin credentials you can use to RDP into the Cityworks server.
[[img/connectioninfo.png]]

Once you have connected to the server, download a Cityworks installer from MyCityworks and run it. Use the RDSInstanceAddress and AztecaPassword values from the Outputs tab in CloudFormation as the database connection info. Select the cityworks database from the drop down.
[[img/install.png]]

Finish the install by running DB Manager against the database.
[[img/dbmanager.png]]

This should complete your Cityworks install and you can now browse the Public DNS name and access your /cityworks application.

## Additional Optional Tasks
These are some additional things you can do to improve your deployemnt.
* Create a custom DNS CNAME record in your domain to point to the Public DNS name from the CityworksInstanceAddress value in the Outputs tab of CloudFormation.
* Install an SSL certificate in IIS for custom DNS name and add a https binding in IIS so you can access the site over https using your domain name.
* Restore an existing SQL bak file to your RDS instance. Follow the guide here https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html  Use the S3 bucket in the value of RDSS3Bucket in the Outputs tab of CloudFormation. The IAM Role and Option group described in the instructions were already created as part of the CloudFormation stack. You can also use the values of S3BucketAccessKeyId and S3BucketSecretAccessKey to access the S3 bucket.

win<h1>AWS Cloud Cost Optimization - Identifying Stale Resources </h1>

In this project, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.

<h2>Description</h2>
The Lambda function fetches all EBS snapshots owned by the same account ('self') and also retrieves a list of active EC2 instances (running and stopped). 
For each snapshot, it checks if the associated volume (if exists) is not associated with any active instance. If it finds a stale snapshot, it deletes it, effectively optimizing storage costs.
<br />

<h2>Resources And Links To Download Required Softwares:</h2>

- <b>aws cost optimization documentation</b> https://www.virtualbox.org/wiki/Downloads](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/cost-and-performance-optimization.html
- <b>Python Knowledge is needed</b>
- <b>Basic Linux Knowledge is needed</b>

<h2>Program walk-through:</h2>

The implementation will be in done in 6 Steps:

STEP 1: Create an EBS volume snapshot of a virtual machine (EC2 instance).

STEP 2: Create a lambda function to delete the virtual machine (EC2 instance).

<p align="center">
 
<h2>Program walk-through:</h2>

STEP 1: Create an EBS volume snapshot of a virtual machine (EC2 instance): <br/>

Here we will create an EC2 instance and then create a snapshot of that instance, which basically an image of the instance.

1.1 EC2 Instance created: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/EC2%20Instance%20created.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Volume will be created along side with the EC2 instance. 

1.2 Volume created along side with the EC2 instance: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Volume%20created%20along%20side%20with%20the%20EC2%20instance2.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

From the EC2 dashboard, we can choose Volume and see the volume created that is associated with our instance.

1.3 Volume created along side with the EC2 instance 2: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Volume%20created%20along%20side%20with%20the%20EC2%20instance.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />
From the EC2 dashboard, we will choose snapshot and create a snapshot of the volume.

1.4 snapshot of the volume created : <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/snapshot%20of%20the%20volume%20created.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

STEP 2: Create a lambda function to delete the virtual machine (EC2 instance).

Will go the lambda dashboard click on create function, choose python as runtime, keep the default permission and create the lambda function.                     

2.1 Lambda function created: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Lambda%20function%20created.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Now we will replace the code in the lambda function by the python code below: The code is built to list all snapshots, list all volumes, list all running EC2 instances. 
Then it will verify if a snapshot belongs to a volume and if that volume is attached to a running EC2 instance, and delete that snapshot if it is not the case.

2.2 ebs_stale_snapshots_python code: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/ebs_stale_snapshots_python%20code.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

After replacing the code, we will save, deploy and then test the function: This is expected to fail as lambda permission is required.

2.3 ebs_stale_snapshots_Test Failed: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/ebs_stale_snapshots_Test%20Failed.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Since lambda function excecution time is 3 seconds by default, let increase it to 10 seconds for this poject. From the lambda function selected, choose configuration Tab and click on edit and chamge the timeout min time to 10.

2.4 Lambda execution time increased: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Lambda%20execution%20time%20increased.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Now let give create an IAM policy to grant permission to list and delete snapshots: From the IAM dashboard, click on create policy, choose EC2 and allow the required permissions
 
2.5 IAM policy created to grant permission to list and delete snapshots : <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/IAM%20policy%20created%20to%20grant%20permission%20to%20list%20and%20delete%20snapshots.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Now let give the required permission for the lambda function: From the lambda function selected, choose configuration Tab and click on permission, click on the role name and then attach the policy created here above
 
2.6 IAM policy attached to the lambda function role name: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/IAM%20policy%20attched%20to%20the%20lambda%20function%20role%20name.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

After the permission granted to the lambda fuunction to list and delete. let run the test again: expected results is to fail due to describe volume and describe instances missing permission

2.7 Test failed due to describe volume and describe instances missing permission: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Test%20failed%20due%20to%20describe%20volume%20and%20describe%20instances%20missing%20permission.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Now let give create an IAM policy (EC2-permission) to describe volume and describe instances snapshots: From the IAM dashboard, click on create policy, choose EC2 and allow the required permissions.
Then, From the lambda function selected, choose configuration Tab and click on permission, click on the role name and then attach the new policy created.
After the new permission granted to the lambda fuunction to list and delete. let run the test again: expected results is to fail due to describe volume and describe instances missing permission
 
2.8 Lambda function got executed this time : <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/ambda%20function%20got%20executed%20this%20time.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

After grating the required permission to delete, decribe snapshot and instances, the lambda function script is executed, but the snapshot will still not be deleted. 
This is expected as the snapshot has its volume associated with the EC2 imstance.

Now let delete our EC2 Instance, this will delete the volume as well, but the snapshot will still be there

2.9 EC2 Instance deleted : <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/EC2%20Instance%20terminated.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

2.10 Volume got deleted after EC2 instance termination : <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Volume%20got%20deleted%20after%20EC2%20instance%20termination.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

2.11 Snapshot did not got deleted after EC2 instance termination : <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Snapshot%20did%20not%20got%20deleted%20after%20EC2%20instance%20termination.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

Now that the EC2 Instance has been deleted along side with the volume, Let run the test of our lambda function again,
this time we are expecting the snapshot to be deleted. we can clearly see the message: "Deleted EBS snapshot snap-06cdbf1bee3e70ad5 as its associated volume was not found"

2.12 Lambda function was able to delete the Snapshot after EC2 instance termination: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Lambda%20function%20was%20able%20to%20delte%20the%20Snapshot%20after%20EC2%20instance%20termination.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

2.13 Snapshot got deleted after EC2 instance termination and lambda function tested again: <br/>

<img src="https://github.com/jpap19/AWS-Cloud-Cost-Optimization/blob/main/Images/Snapshot%20got%20deleted%20after%20EC2%20instance%20termination%20and%20lambda%20function%20tested%20again.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />

CONCLUSION: <br/>

In this project, we have implemented an AWS Cost Optimization by creating a lambda function that deletes each snapshot that has a volume not associated to any running EC2 instance. The goal is to reduce the storage cost.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

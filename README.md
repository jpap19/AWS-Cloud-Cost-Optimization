<h1>AWS Cloud Cost Optimization - Identifying Stale Resources </h1>

In this example, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.

<h2>Description</h2>
The Lambda function fetches all EBS snapshots owned by the same account ('self') and also retrieves a list of active EC2 instances (running and stopped). 
For each snapshot, it checks if the associated volume (if exists) is not associated with any active instance. If it finds a stale snapshot, it deletes it, effectively optimizing storage costs.
<br />

<h2>Resources And Links To Download Required Softwares:</h2>

- <b>aws cost optimization documentation</b> https://www.virtualbox.org/wiki/Downloads](https://docs.aws.amazon.com/wellarchitected/latest/serverless-applications-lens/cost-and-performance-optimization.html
- <b>Python Knowledge is needed</b>
- <b>Basic Linux Knowledge is needed</b>

<h2>Program walk-through:</h2>

<p align="center">
 
The implementation will be in done in 6 Steps:

STEP 1: Create an EBS volume snapshot of a virtual machine (EC2 instance).

 
STEP 1: Create an EBS volume snapshot of a virtual machine (EC2 instance): <br/>

Here we will create an EC2 instance and then create a snapshot of that instance, which basically an image of the instance.

1.1.1 Free Elastic account set up  and Deployment created: <br/>

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/FreeAccountCreated.png" height="150%" width="100%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />
1.2 Installation and setting up of Kali VM: <br/>

Go to the links provided here above, download and install Oracle Virtual Box, then download Kali Linux VM and save in a folder on the computer. virtual box  will be used to setup and run a virtual machine, Kali Linux VM  will be installed on the virtual machine.

Once the installation is complete, log in to the Kali VM using the credentials “kali” for both the username and password.

<img src="https://github.com/jpap19/A-Simple-Elastic-SIEM-Lab/blob/main/Images/KaliSettingUp.png" height="150%" width="150%" alt="AWS Cloud Cost Optimization"/>
<br />
<br />


CONCLUSION: <br/>

In this activity, we have set up a home lab using Elastic SIEM and a Kali VM. We forwarded data from the Kali VM to the SIEM using the Elastic Beats agent, generated security events on the Kali VM using Nmap, and queried and analyzed the logs in the SIEM using the Elastic web interface. We also created a dashboard to visualize security events and then created an alert to detect security events.


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

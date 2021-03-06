Table of Contents
=================

* [Lab Exercises](#lab-exercises)

* [Part I](#part-i)

* [Optional Advanced Exercise – Perform a Storage Performance Test through the CLI](#option1)

* [Exercise 1 - Delphix Engine Configuration](#exercise1)

* [Exercise 2 - Create delphix_disc login on Source and delphix_db login on target](#exercise2)

* [Exercise 3 - Validate Environment with Hostchecker](#exercise3)

* [Exercise 4 – Add a Source and Target Environment](#exercise4)

* [Exercise 5 – Link a dSource](#exercise5)

* [PartII](#part-ii)

* [Exercise 6 – Create and Save a Hook Operation Template](#exercise6)

* [Exercise 7 – Provision a VDB](#exercise7)

* [Exercise 8 – Set a New Retention Policy](#exercise8)

* [Exercise 9 – Refresh a VDB](#exercise9)

* [Exercise 10 – Rewind a VDB](#exercise10)

* [Optional Advanced Exercise – Measure Network Performance through the CLI](#option2)

* [Optional Advanced Exercise - Configure Delphix Replication](#option3)

* [Lab Solutions](#lab-solutions)

* [Exercise 1 – Delphix Engine	Configuration](#exercise1_sol)

* [Exercise 2 – Create	delphix_disc login on Source and delphix_db	login on Target](#exercise2_sol)

* [Exercise 3 – Validate the Source & Target Environments with Hostchecker](#exercise3_sol)

* [Exercise 4 – Add a Source and Target Environment](#exercise4_sol)

* [Exercise 5 – Link a dSource](#exercise5_sol)

* [Exercise 6 – Create and Save a Hook Operation Template](#exercise6_sol)

* [Exercise 7 – Provision a VDB](#exercise7_sol)

* [Exercise 8 – Set a New Retention Policy](#exercise8_sol)

* [Exercise 9 – Refresh a VDB](#exercise9_sol)

* [Exercise 10 – Rewind a VDB](#exercise10_sol)

## <a id="lab-exercises"></a>Lab Exercises
=============

Perform these exercises when instructed by your Delphix Instructor.

## <a id="part-i"></a>Part I
======

## <a id="option1"></a>Optional Advanced Exercise - Perform a Storage Performance Test through the CLI

In this exercise, you will:
-  Log into the Delphix Engine prior to configuration via the Delphix Command Line Interface (CLI)
-  Perform a Storage Test
-  View the Storage Test Results

**Steps**

As an advanced exercise, this lab has no corresponding Lab Solution. Instead, we will walk through the steps to get you acquainted with your lab system and the Delphix CLI.

1. On your Lab Server desktop, double-click on Terminal
2. Type: ssh <sysadmin@10.0.x.10> ('x' is your **Student Number** assigned by your instructor)
    
    a.  If you receive a prompt asking you if you are sure you want to connect, enter: Yes
    
    b.  Enter the password: sysadmin
    
    c.  Escape to the standard CLI prompt by typing: discard
    
    d.  You are now at the root of the Delphix CLI as a System Administrator
3.  Create a storage test by typing: storage test create (see figure !!!)
    
    a.  List the default storage test parameters by typing get
    
    b.  Override the duration and set it to 5 minutes: set duration=5
    
    c.  Begin the storage test by typing: commit
    
    d.  Note that this test will take anywhere between 5-10 minutes to complete

![](./photos/image1.png)

Example Storage Test Configuration

4.  View the storage test results

    a.  Get back to the storage test section of the CLI by typing: storage test list
    
    b.  View a list of completed tests by typing: ls
    
    c.  Type "select" followed by the name of the test from the list. For example: select STORAGE_TEST-1
    
    d.  Enter the result command by typing: result
    
    e.  Then type: commit
    
![](./photos/image2.png)"
 
Example Storage Test Results

## <a id="exercise1"></a>Exercise 1 - Delphix Engine Configuration

In this exercise, you will:
* Access the Delphix Engine GUI for the first time
* Set up the Delphix SYSADMIN user
* Set up the Delphix DELPHIX_ADMIN user
* Configure Timezone Preferences
* Configure Network Settings
* Configure Disks
* Complete the Delphix Engine Configuration

**Steps**
1.  Connect to your Delphix Engine using Firefox on your lab server (see the Important IP Addresses
section of the Getting Started guide above).
2.  Set the new sysadmin password to: delphix
3.  Configure the Delphix Engine with the following details:

    a. NTP on using pool.ntp.org with your local timezone
   
    b. Default network settings
    
    c. Three 5GB volumes in the data pool
    
    d. Default Serviceability options
    
    e. Default Authentication Service options

    f. Appliance marked registered (no support credentials are required for this lab)
    
    g. Completed and saved System setup.

4. Log in with the initial delphix_admin user credentials
5. Set the new delphix_admin password to: delphix
You will know this is successful when you see the main Delphix UI screen with a single group (Untitled) on the
left hand side.

**HOLD - PLACEHOLDER FOR DOCS.DELPHIX LINKS ADMIN/SYSADMIN USER ROLES & SETTING UP THE DELPHIX ENGINE**

## <a id="exercise2"></a>Exercise 2 - Create delphix_disc login on Source and delphix_db login on target

In this exercise, you will:
* Create a Delphix delphix_disc user on your source Sybase Instance
* Create a Delphix delphix_db user on your target Sybase Instance

**Steps**
1.  Use Terminal to SSH into your Linux Source (see the Important IP Addresses section of the Getting
Started guide above).
2.  Execute the following on the source instance
    
    a.  Create delphix_disc user with select on privileges on sysdatabases, sysservers, and
syslisteners:
    
    ```
    isql –Usa –Pdelphixdb –SLINUXSOURCE
    sp_addlogin delphix_disc, delphix_disc
    go
    sp_adduser delphix_disc
    go
    grant select on sysdatabases to delphix_disc
    go
    grant select on sysservers to delphix_disc
    go
    grant select on syslisteners to delphix_disc
    go
    exit
    ```

**Steps**
1.  Use Terminal to SSH into your Linux Target (see the Important IP Addresses section of the Getting
Started guide above).
2.  Execute the following on the target instance

    a.  Create delphix_db user and grant sa_role and sybase_ts_role to delphix_db login

    ```
    isql –Usa –Pdelphixdb –SLINUXTARGET
    sp_addlogin delphix_db, delphix_db
    go
    sp_adduser delphix_db
    go
    sp_role “grant”, sa_role, delphix_db
    go
    sp_role “grant”, sybase_ts_role, delphix_db
    go
    exit
    ```


## <a id="exercise3"></a>Exercise 3 - Validate Environment with Hostchecker

**a. Validate Source Environment with Hostchecker**

In this exercise, you will:
  * Use the ‘hostchecker’ program to run validation tests on your Linux Source

**Steps**
    
    a. Use Terminal to SSH into your Linux Source (see the Important IP Addresses section of the Getting
Started guide above).
 
    b. Go to the hostchecker directory created in the previous exercise and type: ./hostchecker.sh

    c. Run hostchecker for a source and perform the following checks:

        a. Check on the delphix homedir

        b. Check the following ports on your Delphix Engine (10.0.x.10): 22,80,443
    
        c. Check for ssh connectivity
    
        d. Check for sudo privileges as the delphix user
    
        e. Check sshd_config for timeout
    
        f. Check the toolkit path of: **/u01/app/toolkit**

If you have completed all of the checks and they have returned SUCCESS and ALL OK, you have completed
this exercise.

Note: The sshd_config test will return a WARNING response, which is normal in a production installation due
to permissions on the file. If hostchecker is run as root for this test, it will perform the test properly.

**b. Validate the Target Environment with Hostchecker**

In this exercise, you will:
  * Use the ‘hostchecker’ program to run validation tests on your Linux Target
Steps

    a. Use Terminal to SSH into your Linux Target A (see the Important IP Addresses section of the Getting
Started guide above).

    b. Untar the hostchecker_linux_x86.tar file in your home directory

    c. Run hostchecker for a target and perform the following checks:

        a. Check on the delphix homedir

        b. Check the following ports on your Delphix Engine (10.0.x.10): 8415, 22, 111,1110
        
        c. Check for ssh connectivity
        
        d. Check for sudo privileges as the delphix user
        
        e. Check sshd_config for timeout

        f. Check the toolkit path of: /u01/app/toolkit

If you have completed all of the checks and they have returned SUCCESS and ALL OK, you have completed
this exercise.

Note: The sshd_config test will return a WARNING response, which is normal in a production installation due
to permissions on the file. If hostchecker is run as root for this test, it will perform the test properly

## <a id="exercise4"></a>Exercise 4 - Add a Source and Target Environment

In this exercise, you will:

* Connect Delphix to your Source Sybase Server
* Create a Sybase dSource by syncing with your Sybase Source database
* Create a Delphix Group to hold your dSource object
    
**a. Adding Source Environment**

**Steps**

1. Log into the Delphix Engine as delphix_admin
2. Add your Linux Source as an Environment with the following details:
        
    a. Environment Name: Source
        
    b. Host Address: 10.0.x.20 (‘x’ will be your Student Number)
    
    c. OS Username: delphix
        
    d. OS Password: delphix
        
    e. Toolkit Path: /u01/app/toolkit

    f. Check Discover SAP ASE checkbox
        
    g. SAP ASE Discovery credentials

     i. ASE DB Username: delphix_disc
        
     ii. ASE DB Password: delphix_disc

You should now see the Source environment listed under the Environments panel.

**b. Adding a Target Environment**

**Steps**

1. Log into the Delphix Engine as delphix_admin
2. Add your Linux Source as an Environment with the following details:
    a. Environment Name: Target
    
    b. Host Address: 10.0.x.30 (‘x’ will be your Student Number)
    
    c. OS Username: delphix
    
    d. OS Password: delphix
    
    e. Toolkit Path: /u01/app/toolkit
    
    f. SAP ASE Discovery credentials
        
      i. ASE DB Username: delphix_db
        
      ii. ASE DB Password: delphix_db

You should now see the Target environment listed under the Environments panel

## <a id="exercise5"></a>Exercise 5 - Link a dSource

Perform the following steps after the Source and Target Environment is created:
1. SSH to Linux Source as the delphix user

  >Run the following commands:

```
  cd labs
  ./dumpfull_testdb.sh
```
*Special Note*: This lab has a specific configuration that leverages the Target Host as a Staging Host. This is fine for simple use cases. However, for optimal performance, the two should be separate. In order to complete this lab, you must register the Target Host as the Staging Host as well. This is done by accessing the Target Host from the "Environments" tab, selecting LINUXTARGET, and the clicking the "Use as Staging" option accessed via the "edit" icon.

2. From the Delphix admin console click the Source environment on the left side of the screen
3. Click the Databases tab (marked by a large database icon) on the right hand side of your screen
4. Under LINUXSOURCE > testdb, click Add dSource
5. Under the entry testdb, enter the details (case sensitive).
  >The username/password you created in Exercise 2.

    a. Database Username: delphix_disc

    b. DB Password: delphix_disc

    c. Verify Credentials

6. Click Next
7. Keep default dSource Name of testdb
8. Create a new Target Group called DB Source and assign the dSource to the group by clicking on the
group name
9. Go to the Data Management screen and fill in the following information

    c. On Initial Load, choose Most Recent Existing Full Backup

    d. Backup Location, Enter: /u02/sybase_back
    
    e. Staging Environment, choose Target and LINUXTARGET

    f. LogSync, click Enabled

10. Accept defaults for the Hooks
11. Click Finish on the Summary screen

You will know this is successful if the dSource completes in the Actions pane without Errors. Click on Actions
in the top menu bar if you don’t see this pane.

Click on Delphix logo to go to the home screen. The dSource testdb should be listed under the DB Source
group.

## <a id="part-ii"></a>Part II

## <a id="exercise6"></a>Exercise 6 - Create and Save a Hook Operation Template

In this exercise, you will:

* Create a Hook Operation Template called APPUSER
* Insert code into the template that will log into a database and add a user named appuser

**Steps**

1. Create a new Hook Operation Template called: Create APPUSER
    
    a. Select Manage > Hook Templates from the menu
    
    b. Click on the green plus (+) icon
    
    c. Type: Shell Command
    
    d. Contents (enter exactly):
```
$SYBASE/OCS-15_0/bin/isql –Usa –Pdelphixdb –SLINUXTARGET << EOF
sp_addlogin appuser,appuser
go
sp_adduser appuser
go
EOF
```

**IMPORTANT: Make sure the carriage returns you see here are the same in the pasted contents.**

    e. Click the Create button

2. Verify the Hook Operation Template appears in the list
3. Click Close to exit the Operation Template screen

## <a id="exercise7"></a>Exercise 7 - Provision a VDB

In this exercise, you will:

* Create a VDB called devdb
* Use the Hook Operation Template we created previously
* Log into the VDB
* Verify the Hook Operation Template were successful
Steps

1. Select the testdb dSource and Provision a VDB with the following details:
    
    a. Destination Environment: Target
    
    b. Database Name: devdb
    
    c. Truncate Log On Checkpoint: Enabled
    
    d. Configure Clone hook: Create APPUSER
    
    e. VDB Name: devdb

    f. Target Group: DB Target
    
    g. Snapshot Policy: Default Snapshot

2. Complete the VDB creation

  >It may take a couple minutes for the VDB creation to complete. You can monitor the progress on the left hand
  >side of the screen next to the devdb object in the DB Targets group. On the Actions pane on the right hand
  >side of the screen, you should see the Provision virtual database “devdb” item move to the Recently completed
  >pane without error. Once the VDB is created, you can verify that the VDB is operational by:
3. SSH to Linux Target A as the delphix user
4. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
sp_helpdb devdb
go
sp_displaylogin appuser
go
```

This will verify that the VDB is online with the VDB Operation Template we specified, and that the appuser user
was created by our hook.

## <a id="exercise8"></a>Exercise 8 - Set a New Retention Policy

There are four types of Policies in Delphix. In this exercise, you will:
* Create a Retention Policy
* Set the new policy to keep snapshots and logs for 30 days, along with 3 monthly snapshots
* Apply the policy to the VDB we created in the previous exercise

**Steps**

1. Navigate to Manage -> Policies
2. Create a new Retention policy for devdb with the following details:

    a. Name: Long Term

    b. 30 days of snapshot and log retention
    
    c. 3 monthly snapshots taken on the 1st of the month
    
## <a id="exercise9"></a>Exercise 9 - Refresh a VDB

In this exercise, you will:

* Create a new table on your source database
* Snapshot the dSource
* Refresh your VDB
* Verify the new table appears on the VDB

**Steps**

1. Connect to your Linux Source server as the delphix user via SSH
2. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXSOURCE
use testdb
go
create table t1 ( comments varchar(100))
go
insert into t1 values (‘test data’)
go
exit
```

Take a transaction log dump on testdb
```
cd /home/delphix/labs
./dumptran_testdb.sh
```

3. Go back to the Delphix Engine
4. Wait for a minute or two for Delphix Engine to ingest the new transaction log dump.

Note: A new dSource card should appear after the transaction log has been ingested

5. Select the devdb VDB and click the small Open button ( )
6. Refresh the devdb VDB using the latest snapshot from the testdb dSource

Once the refresh has completed, you can log into devdb to confirm.

7. Connect to your Linux Target A server as the delphix user via SSH
8. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
select * from t1
go
```

If this returns a count of row, the snapshot/refresh was successful.

## <a id="exercise10"></a>Exercise 10 - Rewind a VDB

In this exercise, you will:

* Take a snapshot of the devdb VDB
* Drop a table in the devdb VDB
* Rewind the devdb VDB to recover from the action

**Steps**

1. Connect to your Linux Target A server as the delphix user via SSH
2. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
insert into t1 values (‘before rewind’)
go
```

3. Take a snapshot of the devdb VDB
4. Run the following commands

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
drop table t1
go
```

We just dropped t1 table. Now we will rewind the VDB to that last good snapshot to fix this.

5. Select the devdb VDB
6. Select the snapshot card associated with the date/time you recorded prior to corrupting your database.
7. Rewind the VDB to the snapshot card you took prior to the corruption.
Once the rewind operation is complete, you can confirm the rewind was successful by connecting to the server

again and querying the database:

8. Connect to your Linux Target A server as the delphix user via SSH
9. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
select * from t1
go
```

The count should return 2 rows, and the database is online.

## <a id="option2"></a>Optional Advanced Exercise - Measure Network Performance through the CLI

In this exercise, you will:
* Log into the Delphix Engine CLI as delphix_admin
* Perform a network latency test to TargetA
* Perform a network throughput test to TargetA

**Steps**

As an advanced exercise, this lab has no corresponding Lab Solution. Instead, we will walk through the steps
to get you acquainted the Delphix CLI for delphix_admin.

1. On your Lab Server desktop, double-click on Terminal
2. Type: ssh delphix_admin@10.0.x.10 (‘x’ is your Student Number assigned by your instructor)

    a. If you receive a prompt asking you if you are sure you want to connect, enter: Yes

    b. Enter the password: delphix

    c. You are now at the root of the Delphix CLI as a Delphix Administrator

3. Create a network latency test by typing: network test latency create
    
    a. List the default/required parameters by typing: get
    
    b. Set the remoteHost value to the TargetA environment IP address: set remoteHost=10.0.x.30 (‘x'
will be your Student Number)

    c. Begin the test by typing: commit

![](./photos/image3.png)
 Example Network Latency Test Submission
 
4. View the results of the latency test:
    a. Get to the latency test section again by typing: network test latency

    b. List the completed tests by typing: ls

    c. Type “select” followed by the name of the test from the list. For example: select 10.0.1.30-2015-
09-18T12:47:19.711Z

    d. View the results of the test by typing: get

 ![](./photos/image4.png)
Example Latency Test Results

5. Create a network throughput test

    a. While still logged into the CLI, return to the root by typing: cd /

    b. Begin a network throughput test by typing: network test throughput create

    c. List the default/required parameters by typing: get

    d. Set the remoteHost value to the TargetA environment IP address: set remoteHost=10.0.x.30 (‘x’
will be your Student Number)
    
    e. Begin the test by typing: commit
    
![](./photos/image5.png)
Example Network Throughput Test Submission

6. View the results of the throughput test:
    
    a. Get to the throughput test section again by typing: network test throughput
    
    b. List the completed tests by typing: ls
    
    c. Type “select” followed by the name of the test from the list. For example: select 10.0.1.30-2015-
09-18T13:13:08.152Z

    d. View the results of the test by typing: get

 ![](./photos/image6.png)
Example Network Throughput Test Results

## <a id="option3"></a>Optional Advanced Exercise - Configure Delphix Replication

Note: This exercise is only possible if your classroom has been configured with 2 or more students.

In this exercise, you will:
* Set up a replication profile
* Replicate your entire Delphix Engine to another Delphix Engine
* View the replicas in the target Delphix Engine

**Steps**

As an advanced exercise, this lab has no corresponding Lab Solution. Instead, we will walk through the steps
to get you acquainted the Delphix Replication capability.

1. In the Delphix GUI, select System and then Replication on the top menu bar
2. Add a Replication Profile called DR Replica
    
    a. Click the plus sign next to Replication Profiles on the top left

    b. Enter a Replica Profile Name: DR Replica
    
    c. For Target Engine, enter the Delphix Engine IP address for the next student in your classroom
environment. If you are the last student, use the Delphix Engine IP address for Student 1. For
example, in a class with 3 students:
    
      i. Student 1 Delphix Engine is at 10.0.1.10, and they will replicate to 10.0.2.10
      ii. Student 2 Delphix Engine is at 10.0.2.10, and they will replicate to 10.0.3.10
      iii. Student 3 Delphix Engine is at 10.0.3.10, and they will replicate to 10.0.1.10
      iv. Ask your instructor if you have any questions or confusion about this configuration.

    d. Enter the User Name: delphix_admin
  
    e. Enter the Password: delphix

    f. Do not enable Automatic Replication or configure Traffic Options

    g. For the Objects Being Replicated, select: Entire Delphix Engine
  
    h. Click Create at the bottom when ready.

3. Start the Replication by clicking the Replicate Now button on the top right of your screen.
4. Click Replicate to confirm you are ready to begin.
5. Once the initial full replication is complete, you will see a message stating “Last Replication
Successful.”

6. Check the results on your target Delphix Engine
  
    a. In your lab server browser, enter the IP address you used for the Target Engine in your replica
profile. For example, if you are Student 1, your Delphix Engine is at 10.0.1.10, and your target
would have been 10.0.2.10.
  
    b. Log in as user delphix_admin with the password delphix
Lab Exercises 21 © 2015 Delphix Corp. All rights reserved

    c. Observe the dropdown list next to Databases on the top left corner of your screen. It currently
says Available which is the default Namespace for Delphix replica targets.

    d. In order to see the replica objects, click on the dropdown list and select the second entry, which
should be the IP address of the Source Delphix Engine that sent the replica.

 
7. While still logged into your target Delphix Engine, click on System and then Replication
8. Observe the Received Replicas section at the bottom, indicating and verifying the target’s receipt of
replication data.
    a. Note: The Failover Now option will not work for these labs unless all objects on the target
engine are deleted due to object name collisions. This is an inherent outcome to plan for when
using Active/Active replication. By sending/receiving replication data, this concludes the
Replication exercise. 

# <a id="lab-solutions"></a>Lab Solutions

## <a id="exercise1_sol"></a> Exercise 1 – Delphix Engine Configuration

1. Connect to your Delphix Engine using Firefox on your lab server. The IP address will be 10.0.x.10,
where “x” is your Student Number.
2. Set an email address and new password for the sysadmin user and click Next.
3. Configure Delphix to use NTP time with the pool.ntp.org NTP server. Change the timezone to your local
timezone. Then click Next.
4. Review the network configuration and click Next. Do not make any changes to this section.
5. Review the disk layout and click Next. Do not make any changes to this section.
6. Review the Serviceability options and click Next. Do not make any changes to this section.
7. Review the Authentication Service options and click Next. Do not make any changes to this section.
8. Click Mark Appliance Registered
9. Review the summary and click Finish to complete the System configuration.
10. Click Yes to save the configuration.
11. Click OK to acknowledge completion
12. Log in with credentials:
    
    a. Username: `delphix_admin`

    b. Password: `delphix`

13. Set an email address and new password for the delphix_admin user

## <a id="exercise2_sol"></a> Exercise 2 – Create delphix_disc login on Source and delphix_db login on Target
a. Create the “delphix_disc” User on Source Sybase Instance
In this exercise, you will:
* Create a Delphix delphix_disc user on your source Sybase Instance.

**Steps**

1. Use Terminal to SSH into your Linux Source (see the Important IP Addresses section of the Getting
Started guide above).
2. Login to LINUXSOURCE instance
    
    a. Create delphix_disc user with select on privileges on sysdatabases, sysservers, and
syslisteners.

```
isql –Usa –Pdelphixdb –SLINUXSOURCE
sp_addlogin delphix_disc, delphix_disc
go
sp_adduser delphix_disc
go
grant select on sysdatabases to delphix_disc
go
grant select on sysservers to delphix_disc
go
grant select on syslisteners to delphix_disc
go
exit
```

![](./photos/image7.png)

    b. Create the "delphix_db" user on Target Sybase Instance

In this exercise, you will:
• Create a Delphix delphix_disc user on your source Sybase Instance.

**Steps**

1. Use Terminal to SSH into your Linux Source (see the Important IP Addresses section of the Getting
Started guide above).
2. Login to TARGET instance
  
    a. Grant sa_role to delphix_db login

```
isql –Usa –Pdelphixdb –SLINUXTARGET
sp_addlogin delphix_db, delphix_db
go
sp_adduser delphix_db
go
sp_role “grant”, sa_role, delphix_db
go
exit
```

![](./photos/image8.png)

## <a id="exercise3_sol"></a> Exercise 3 – Validate the Source Environment with Hostchecker

Validate Source Environment with Hostchecker

1. Connect to your Linux Source by opening Terminal on your Lab Server and running: conn 10.0.x.20 (‘x’
will be your **Student Number**).
2. Ensure you are inside the hostchecker location with the command: `cd /home/delphix/hostchecker`
3. Run hostchecker.jar with the command: ./hostchecker.sh
4. Indicate that this machine is a “source” by typing: source
5. Review the available checks that can be run on this system
6. Type “1” and press Enter. The script will check homedir permissions and return SUCCESS and ALL
OK.
7. Type “3” and press Enter.
  
    a. Enter an IP address of: 10.0.x.10 (‘x’ will be your Student Number).

    b. Enter the port 22

    c. The script will test the port and return SUCCESS and ALL OK.

8. Repeat the previous step for the following ports: 80,443
9. Type “8” and press Enter.
  
    a. Enter a password of: `delphix`

    b. The script will test the SSH connectivity to the host and return SUCCESS and ALL OK.

10. Type “9” and press Enter.
  
    a. The script will return a WARNING due to permissions. This is normal.

11. Type “10” and press Enter.
  
    a. Enter a password of: `delphix`
  
    b. The script will test sudo privileges and return SUCCESS and ALL OK

12. Type “11” and press Enter.

    a. Enter a path of: /u01/app/toolkit

    b. The script will test the path and return SUCCESS and ALL OK

13. Type “quit” to exit hostchecker.

### Validate the Target Environment with Hostchecker

1. Connect to your Linux Target A by opening Terminal on your Lab Server and running: conn 10.0.x.30
(‘x’ will be your Student Number).
2. Observe the name and location of the hostchecker package by running the command: ls -ltr
3. Expand the hostchecker package by running: tar -xvf hostchecker_linux_x86.tar
4. Type: cd hostchecker
5. Run hostchecker.jar with the command: ./hostchecker.sh
6. Indicate that this machine is a “target” by typing: target
7. Review the available checks that can be run on this system
8. Type “1” and press Enter. The script will check homedir permissions and return SUCCESS and ALL
OK.
9. Type “3” and press Enter.
    
    a. Enter an IP address of: 10.0.x.10 (‘x’ will be your Student Number).
    
    b. Enter the port: 8415
    
    c. The script will test the port and return SUCCESS and ALL OK.

10. Repeat the previous step for the following ports: 22,111,1110
11. Type “8” and press Enter.
    
    a. The script will return WARNING due to permissions. This is normal.

12. Type “9” and press Enter.
    
    a. Enter a password of: delphix
    
    b. The script will test sudo privileges and return SUCCESS and ALL OK

13. Type “10” and press Enter.
    
    a. Enter a path of: /u01/app/toolkit

    b. The script will test the path and return SUCCESS and ALL OK

14. Type “quit” to exit hostchecker.

## <a id="exercise4_sol"></a>Exercise 4 – Add a Source and Target Environment

### Adding a Source Environment

1. If you are not already logged in, login to your Delphix Engine as delphix_admin using the password you
set in the last exercise.
2. In the top menu bar, click Manage and then Environments. 
![](./photos/image9.png)
3. Click the plus sign next to the word Environments on the top left portion of your screen under the
Delphix logo.
4. Enter the following details:
    
    a. Environment Name: Source
    
    b. Host Address: 10.0.x.20 (‘x’ will be your Student Number)
    
    c. OS Username: delphix
    
    d. OS Password: delphix
    
    e. Toolkit Path: /u01/app/toolkit
    
    f. SAP ASE Discovery credentials
    
      i. ASE DB Username: delphix_disc

      ii. ASE DB Password: delphix_disc
![](./photos/image10.png)
![](./photos/image11.png)

5. Click OK
6. Wait for the Environment to be created.

### Adding a Target Environment
1. If you are not already logged in, login to your Delphix Engine as delphix_admin using the password you
set in the last exercise.
2. In the top menu bar, click Manage and then Environments.
3. Click the plus sign next to the word Environments on the top left portion of your screen under the
Delphix logo.
4. Enter the following details:
    a. Environment Name: `Target`
    
    b. Host Address: 10.0.x.30 (‘x’ will be your Student Number)

    c. OS Username: `delphix`
    
    d. OS Password: `delphix`

    e. Toolkit Path: `/u01/app/toolkit`

    f. SAP ASE Discovery credentials

      i. ASE DB Username: delphix_db
        
      ii. ASE DB Password: delphix_db
![](./photos/image12.png)
![](./photos/image13.png)

5. Click OK
6. Wait for the Environment to be created.


## <a id="exercise5_sol"></a>Exercise 5 – Link a dSource

Perform the following steps after the Source and Target Environment is created:

1. SSH to Linux Target A as the delphix user
Run the following commands:

```
cd /home/delphix/labs
./dumpfull_testdb.sh
```
![](./photos/image14.png)

Special Note: This lab has a specific configuration that leverages the Target Host as a Staging Host. This is fine for simple use cases. However, for optimal performance, the two should be separate. In order to complete this lab, you must register the Target Host as the Staging Host as well. This is done by accessing the Target Host from the "Environments" tab, selecting LINUXTARGET, and the clicking the "Use as Staging" option accessed via the "edit" icon.
![](./photos/image19.png)
![](./photos/image20.png)

2. Click the Source environment on the left side of the screen

3. Click the Databases tab (marked by a large database icon) on the right hand side of your screen
![](./photos/image15.png)

4. Under LINUXSOURCE > testdb, click Add dSource
![](./photos/image16.png)

5. On dSource Name field, enter testdb
6. Under the entry testdb, enter the details (case sensitive).
The username/password you created in Exercise 2.
    a. Database Username: delphix_disc

    b. DB Password: delphix_disc

    c. Verify Credentials

![](./photos/image17.png)

7. Click Next
8. Create a new Group called: DB Source
![](./photos/image18.png)

9. Click Next
10. Fill up the following information on this page
    a. On Initial Load, choose Most Recent Existing Full Backup
    
    b. Backup Location, Enter: /u02/sybase_back

    c. Staging Environment, choose Target and LINUXTARGET

    d. LogSync, click Enabled
![](./photos/image21.png)

11. Accept defaults for the Hooks
12. Click Next
13. Click Finish
![](./photos/image22.png)

You will know this is successful if the dSource completes in the Actions pane without Errors. Click on Actions
in the top menu bar if you don’t see this pane. 

Click on Delphix logo to go to the home screen. The dSource testdb should be listed under the DB Source
group.

## <a id="exercise6_sol"></a> Exercise 6 – Create and Save a Hook Operation Template

1. In the top menu bar, click Manage, then Hook Templates
![](./photos/image30.png)

2. Click the plus sign under the word Templates in the Hook Operation Templates Wizard
3. Provide the Name: Create APPUSER
4. Ensure Type is set to: Shell Command
5. Under Contents, enter the following code:

```
$SYBASE/OCS-15_0/bin/isql –Usa –Pdelphixdb –SLINUXTARGET << EOF
sp_addlogin appuser, appuser
go
sp_adduser appuser
go
EOF
```
![](./photos/image31.png)

6. Click Create
7. Verify the Hook Operation Template is in the list, then click Close

## <a id="exercise7_sol"></a> Exercise 7 – Provision a VDB
In 1this exercise, you will:
* Create a VDB called devdb
* Use the VDB Configuration Template we created previously
* Use the Hook Operation Template we created previously
* Log into the VDB
* Verify the VDB Configuration Template and Hook Operation Template were successful

**Steps**

1. In the DB Source folder, click testdb object
![](./photos/image23.png)

2. Click Provision. The Provision VDB wizard will open. 
![](./photos/image24.png)

3. Click on Target on the left side of the screen
![](./photos/image25.png)

4. On Database Name, type devdb
5. Click on Truncate Log On Checkpoint checkbox
![](./photos/image26.png)

6. Click Next
7. Click the plus sign to the right of Target Group, and enter the Group Name: DB Targets
8. Click Next
9. With Configure Clone already selected on the left side of the Provision VDB Wizard, click the plus sign
on the right hand side of the wizard
10. Click the icon with an arrow pointing into a box with the tooltip Import hook operation template
11. Click on the Create APPUSER template we created earlier, then click Import
![](./photos/image27.png)
![](./photos/image28.png)
![](./photos/image29.png)

12. Click Next
13. Verify the summary information, and click Finish

It may take a couple minutes for the VDB creation to complete. You can monitor the progress on the left hand
side of the screen next to the devdb object in the DB Targets group. On the Actions pane on the right hand
side of the screen, you should see the Provision virtual database “devdb” item move to the Recently completed
pane without error. Once the VDB is created, you can verify that the VDB is operational by:

14. SSH to Linux Target A as the delphix user
15. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
sp_helpdb devdb
go
sp_displaylogin appuser
go
```
![](./photos/image32.png)

This will verify that the VDB is online with the VDB Configuration Template we specified, and that the appuser
user was created by our hook.

## <a id="exercise8_sol"></a> Exercise 8 – Set a New Retention Policy
1. In the top menu bar, click on Manage and then Policies
![](./photos/image33.png)

2. Click on Modify Policy Templates
3. Under the Retention column, click Apply New Policy
![](./photos/image34.png)

4. Provide the following details:
    
    a. Name: Long Term
    
    b. Keep Logs for: 30 days
    
    c. Keep Snapshots for: 30 days
![](./photos/image35.png)

5. Click Advanced
6. Click the checkbox next to Keep # Monthly(s)
7. Click OK
8. Click the Back to Policy Management button
9. Click on the cell on the devdb row, Retention column. It should currently say Default Retention
![](./photos/image36.png)

10. Click Long Term in the list that pops up
![](./photos/image37.png)

## <a id="exercise9_sol"></a>Exercise 9 – Refresh a VDB

In this exercise, you will:
* Create a new table on your source database
* Snapshot the dSource
* Refresh your VDB
* Verify the new table appears on the VDB

**Steps**

1. Connect to your Linux Source server as the delphix user via SSH
2. Run the following commands:

```
cd /home/delphix/labs
isql –Usa –Pdelphixdb –SLINUXSOURCE
use testdb
go
create table t1 ( comments varchar(100))
go
insert into t1 values (‘test data’)
go
exit
```
![](./photos/image38.png)

Take a transaction log dump on devdb
`./dumptran_testdb.sh`
![](./photos/image39.png)

3. Go back to the Delphix Engine
4. Wait for a minute or two for Delphix Engine to ingest the new transaction log dump.
5. Select the devdb VDB and click the small Open button
6. Refresh the devdb VDB using the latest snapshot from the testdb dSource
![](./photos/image40.png)
![](./photos/image41.png)
![](./photos/image42.png)

7. Connect to your Linux Target A server as the delphix user via SSH
8. Run the following commands:

```
use devdb
go
select * from t1
go
```
![](./photos/image43.png)

If this returns a count of row, the snapshot/refresh was successful.

## <a id="exercise10_sol"></a>Exercise 10 – Rewind a VDB

In this exercise, you will:
* Take a snapshot of the devdb VDB
* Drop a table in the devdb VDB
* Rewind the devdb VDB to recover from the action

**Steps**

1. Connect to your Linux Target A server as the delphix user via SSH
2. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
insert into t1 values (‘before rewind’)
go
```
![](./photos/image44.png)

3. Take a snapshot of the devdb VDB
    a. Under Database, expand DB Target folder.
    
    b. Under devdb VDB, click camera icon to take a snapshot
![](./photos/image45.png)


4. Run the following commands

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
drop table t1
go
```
![](./photos/image46.png)

We just dropped t1 table. Now we will rewind the VDB to that last good snapshot to fix this.

5. Select the devdb VDB
6. Select the snapshot card associated with the date/time you recorded prior to dropping the table in your
database.
7. Rewind the VDB to the snapshot card you took prior to the corruption
    a. Click on Rewind VDB button.

    b. Click Yes
![](./photos/image47.png)
![](./photos/image48.png)
![](./photos/image49.png)

Once the rewind operation is complete, you can confirm the rewind was successful by connecting to the server
again and querying the database:

8. Connect to your Linux Target A server as the delphix user via SSH
9. Run the following commands:

```
isql –Usa –Pdelphixdb –SLINUXTARGET
use devdb
go
select * from t1
go
```
![](./photos/image50.png)

The count should return 2 rows, and the database is online.

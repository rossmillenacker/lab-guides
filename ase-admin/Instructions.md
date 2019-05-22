Table of Contents
=================

* [Lab Exercises](#lab-exercises)

* [Part I](#part-i)

* [Optional Advanced Exercise – Perform a Storage Performance Test through the CLI](#Optional-Advanced-Exercise-Perform-a-Storage-Performance-Test-through-the-CLI)

* [Exercise 1 - Delphix Engine Configuration](#Exercise-1---Delphix-Engine-Configuration)

* [Exercise 2 - Create delphix_disc login on Source and delphix_db login on target](#Exercise-2-Create-delphix_disc-login-on-Source-and-delphix_db-login-on-target)

* [Exercise 3 - Validate Environment with Hostchecker](#Exercise-3-Validate-Environment-with-Hostchecker)

* [Exercise 4 – Add a Source and Target Environment](#Exercise-4-Add-a-Source-and-Target-Environment)

* [Exercise 5 – Link a dSource](#Exercise-5-Link-a-dSource)

* [PartII](#part-ii)

* [Exercise 6 – Create and Save a Hook Operation Template](#Exercise-6-Create-and-Save-a-Hook-Operation-Template)

* [Exercise 7 – Provision a VDB](#Exercise-7-Provision-a-VDB)

* [Exercise 8 – Set a New Retention Policy](#Exercise-8-Set-a-New-Retention-Policy)

* [Exercise 9 – Refresh a VDB](#Exercise-9-Refresh-a-VDB)

* [Exercise 10 – Rewind a VDB](#Exercise-10-Rewind-a-VDB)

* [Optional Advanced Exercise – Measure Network Performance through the CLI](#Optional-Advanced-Exercise-Measure-Network-Performance-through-the-CLI)

Lab Exercises
=============

Perform these exercises when instructed by your Delphix Instructor.

Part I
======
Optional Advanced Exercise - Perform a Storage Performance Test through the CLI
-------------------------------------------------------------------------------

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

"![](./images/image1.png)"
Example Storage Test Configuration

4.  View the storage test results

    a.  Get back to the storage test section of the CLI by typing: storage test list
    
    b.  View a list of completed tests by typing: ls
    
    c.  Type "select" followed by the name of the test from the list. For example: select STORAGE_TEST-1
    
    d.  Enter the result command by typing: result
    
    e.  Then type: commit
    
 "[](./images/impage2.png)"
 Example Storage Test Results

Exercise 1 - Delphix Engine Configuration
-----------------------------------------

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

Exercise 2 - Create delphix_disc login on Source and delphix_db login on target
-------------------------------------------------------------------------------

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


Exercise 3 - Validate Environment with Hostchecker
--------------------------------------------------
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

Exercise 4 - Add a Source and Target Environment
------------------------------------------------

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

Exercise 5 - Link a dSource
---------------------------

Perform the following steps after the Source and Target Environment is created:
1. SSH to Linux Target A as the delphix user

  >Run the following commands:

```
  cd /home/delphix/labs
  ./dumpfull_testdb.sh
```

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

Exercise 6 - Create and Save a Hook Operation Template
------------------------------------------------------

In this exercise, you will:

* Create a Hook Operation Template called APPUSER
* Insert code into the template that will log into a database and add a user named appuser

**Steps**

1. Create a new Hook Operation Template called: Create APPUSER
    
    a. Select Manage > Operation Templates from the menu
    
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

Exercise 7 - Provision a VDB
----------------------------

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

Exercise 8 - Set a New Retention Policy
---------------------------------------

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
    
Exercise 9 - Refresh a VDB
--------------------------

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

Exercise 10 - Rewind a VDB
--------------------------

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

Optional Advanced Exercise - Measure Network Performance through the CLI
------------------------------------------------------------------------

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

 `![](./images/image12.png) - latency test submission example image 3`
 Example Network Latency Test Submission
 
4. View the results of the latency test:
    a. Get to the latency test section again by typing: network test latency

    b. List the completed tests by typing: ls

    c. Type “select” followed by the name of the test from the list. For example: select 10.0.1.30-2015-
09-18T12:47:19.711Z

    d. View the results of the test by typing: get

 `![](./images/image12.png) - latency test results example image 4`
Example Latency Test Results

5. Create a network throughput test

    a. While still logged into the CLI, return to the root by typing: cd /

    b. Begin a network throughput test by typing: network test throughput create

    c. List the default/required parameters by typing: get

    d. Set the remoteHost value to the TargetA environment IP address: set remoteHost=10.0.x.30 (‘x’
will be your Student Number)
    
    e. Begin the test by typing: commit
    
 `![](./images/image12.png) - throughput test submission example image 5`
Example Network Throughput Test Submission

6. View the results of the throughput test:
    
    a. Get to the throughput test section again by typing: network test throughput
    
    b. List the completed tests by typing: ls
    
    c. Type “select” followed by the name of the test from the list. For example: select 10.0.1.30-2015-
09-18T13:13:08.152Z

    d. View the results of the test by typing: get

 `![](./images/image12.png) - throughput test results example image 6`
Example Network Throughput Test Results

Optional Advanced Exercise - Configure Delphix Replication
----------------------------------------------------------

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

 `![](./images/image12.png) - Replication Success Screen - Image 7`
 Example Successful Replication
 
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

 `![](./images/image12.png) - Example Replica Namespace View - Image 8`
 Example Replica Namespace View
 
7. While still logged into your target Delphix Engine, click on System and then Replication
8. Observe the Received Replicas section at the bottom, indicating and verifying the target’s receipt of
replication data.
  a. Note: The Failover Now option will not work for these labs unless all objects on the target
engine are deleted due to object name collisions. This is an inherent outcome to plan for when
using Active/Active replication. By sending/receiving replication data, this concludes the
Replication exercise. 


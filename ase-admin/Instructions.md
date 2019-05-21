Table of Contents
=================

* [Lab Exercises](#lab-exercises)

* [Part I](#part-i)

* [Optional Advanced Exercise – Perform a Storage Performance Test through the CLI](#Optional-Advanced-Exercise-Perform-a-Storage-Performance-Test-through-the-CLI)

* [Exercise 1 - Delphix Engine Configuration](#Exercise-1-Delphix-Engine-Configuration)

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


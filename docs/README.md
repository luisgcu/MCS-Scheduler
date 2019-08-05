# Scheduler for M70x/M600
### Introduction

Some drive applications require  the motor operation during certain period of time  during and specific week   days .   For example is  pretty common in some application user want to avoid motor operation during peak hours ( for non critical applications)  during peak time the electricity tariff usually are higher than the normal hourly operation.

Within our AC drive portfolio  we don't have   any that can do time based start/stop schedule.  Fortunately we have available RTC keypad that can be use as  real time source to implement  scheduled  start/stop of a motor controlled by the drive .  The Keypad by itself is unable to do the scheduler that we need, but our drives has a internal PLC can be used to write a piece of code  that can run  the logic for the hourly control required. 

### Hardware Requirements.

- AC drives M70x or M600.
- KI-Keypad-RTC or Remote Keypad RTC.
- M400/C200 can be used with Remote Keypad RTC ( Not tested yet).

### Software Requirements.

- Machine Control Studio v1.8.0 [LINK](http://www.controltechniques.com/CTDownloads/SharePoint/Download.aspx?SiteID=4&ProductID=150&DownloadID=5149&VersionID=7589)

- Connect Drive Commissioning Software  [LINK](http://www.controltechniques.com/CTDownloads/SharePoint/Download.aspx?SiteID=4&ProductID=150&DownloadID=6041&VersionID=8669)

  

------

### How it works.

Scheduler has 3 instances available that are  can be used   separated .  The picture below illustrate the option for scheduler2 .

![Single Instance](https://github.com/luisgcu/MCS-Scheduler/blob/master/docs/Scheduler%20instance.jpg)

**For the scheduler to work need to be enable first, it also require the user select the days of the week  required the scheduler to operate .**

**Scheduler 1 enable** : M18.031  |  **Scheduler 1 weeks day enable** M18.032 {Monday} ...M18.038{Sunday}

**Scheduler 2 enable** : M18.039  |  **Scheduler 2 weeks day enable** M18.040{Monday} ...M18.046{Sunday}

**Scheduler 3 enable** :  M19.031 | **Scheduler 3 weeks day enable** M19.032{Monday} ...M19.046{Sunday}

The next setup required is to set the desire hour we want the drive to turn on the motor and the hour to turn off.  In this case we just use the hour to setup the hourly schedule. 

**Scheduler 1 hour to start motor** : M18.011 | **Scheduler 1 hour to stop motor** :M18.012

**Scheduler 2 hour to start motor** : M18.013 | **Scheduler 2 hour to stop motor** :M18.014

**Scheduler 3 hour to start motor** : M18.015 | **Scheduler 3 hour to stop motor** :M18.016

The 3 scheduler instances are combined by a logic OR  as we show below .  The OR output is linked to M18.050.

![The 3 Scheduler Instances](https://github.com/luisgcu/MCS-Scheduler/blob/master/docs/Scheduler%20general%20view.jpg)

**Macro that set the schedulers output command to the  drive RunFwd**.

We have a macro within the MCS program that setup some  of the drive parameters  to be able to link the schedulers output  [M18.P050]  to the drive Run forward command [M06.P030].

The Parameter that activate that macro is [M18.P029] the value to have the macro run by the PLC is = 1234 the macro runs once in the PLC.

```c
//macro to setup the  drive to start stop based on the scheduler
IF (gvl.enbl_mcr =1234 AND first_shoot=TRUE) THEN   // gvl.enbl_mcr 	:= M18.P029; 	
	M08.P022	:=	0.00; 		//clear destination [default is M06.030] RunFwd
	M09.P004	:=	18.050;   	// Logic Fucntion 1 source1-->Asigns the Schduler ouput to the  funtion 
    M09.P006    :=  0.00;	    // Logic Function 1 Source 2-->Make sure the source is clear
	M09.P007    :=  TRUE;       // Logic Function 1 Source 2 invert
	M09.P008    :=  FALSE;      // Logic Function 1 Output invert. 
	M09.P010    :=  6.030;      // Logic Funtion 1 Destination  set to Pr [6.030] RunFwd
	M10.P000	:=	1001;    	//Save Parameters
	M10.P038	:= 	100; 	 	//Reset Drive
	Wait_Time(T#400MS);         //Delay for 400ms after save and reset. 
	first_shoot := FALSE;       //Run this  section once once	
END_IF

```

[Logic Function 1 setup by the Macro](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/Logic%20Function%201.jpg)

------

###  Setup the Scheduler using Unidrive M Connect.

**1-** Clone this repository to a location in  your PC, to do that  just click "clone or download"  and the click download  as ZIP. 

![](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/Download%20repository.jpg)

**2-** Unzip de content a known location in your PC.

![](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/binfiles%20folder.jpg)

**3-** The binfiles_drives folder have the binary file compiled for M600/700/701, using the Unidrive M connect upload the PLC program.

**4-**  With   unidrive M connect you will be able to download the scheduler PLC program to the drive. Make sure you pick the correct binary file name that match the drive you are connected. 

![DEPLOY USER PROG](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/DeployUserProgram%20Mconnect.jpg)

**5-** For easy setup of the scheduler when  unidrive   M connect is used  we provide 2 files that are required to be copied into your Drive M connect project, the files are inside  [MConnect folder ](https://github.com/luisgcu/RTC-Scheduler/tree/master/Mconnect) of this repository,  these  files are.

- **Scheduler.macro** , Copy the file  to your drive Mconnect Macro folder,  to do that on the Unidrive  M connect ,  right click over the Macro Files folder and then click add files, when the  Macro is successfully added to project you will have the option to download the macro to the drive ( if you are connected  or to load the values if your are working offline), the main reason to  use the macro is because the application menu register's listed on it has the short names used on the  application menu registers 18 and 19.

  ![MACRO ](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/MacroFile.jpg) .

- **Scheduler.customlist** , Copy the file into your drive Mconnect project "custom list folder", to do it just  right click over the  custom list folder and select add files (look for path where you had  unzip the repository) the file to add is **Scheduler.customlist**, after successfully added it you can open it and if your are connected to the drive you can start editing the scheduler parameters.

  ![Custom list ](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/CustomList.jpg).

  

  







 










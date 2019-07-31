# Scheduler for M70x/M600
### Introduction

Some drive applications require  the motor operation during certain hour range during a  day  and the week.  Is pretty common in some application user want to avoid motor operation during peak hours ( for non critical applications)  because the electricity tariff are higher than the normal hourly operation.

Within our AC drive portfolio  we don't have   any that can do time based start/stop schedule.  Fortunately we have available RTC keypad that can be use as  real time source to implement  scheduled  start/stop of a motor.  The Keypad by itself is unable to do the scheduler that we need, but our drives has a internal PLC can be used to write a piece of code  that can has the logic for the hourly control required. 

### Hardware Requirements.

- AC drives M70x or M600.
- KI-Keypad-RTC or Remote Keypad RTC.
- M400/C200 can be used with Remote Keypad RTC ( Not tested yet).

### Software Requirements.

- Machine Control Studio v1.8.0 [LINK](http://www.controltechniques.com/CTDownloads/SharePoint/Download.aspx?SiteID=4&ProductID=150&DownloadID=5149&VersionID=7589)
- Connect Drive Commissioning Software  [LINK](http://www.controltechniques.com/CTDownloads/SharePoint/Download.aspx?SiteID=4&ProductID=150&DownloadID=6041&VersionID=8669)

### How it works.

Scheduler has 3 instances available that are  can be setup  separated .  The picture below illustrate the option for scheduler2 .

![Single Instance](https://github.com/luisgcu/MCS-Scheduler/blob/master/docs/Scheduler%20instance.jpg)

**For the scheduler to works need to be enable first along with the week day required to work.**

**Scheduler 1 enable** : M18.031  |  **Scheduler 1 weeks day enable** M18.032 {Monday} ...M18.038{Sunday}

**Scheduler 2 enable** : M18.039  |  **Scheduler 2 weeks day enable** M18.040{Monday} ...M18.046{Sunday}

**Scheduler 3 enable** :  M19.031 | **Scheduler 3 weeks day enable** M19.032{Monday} ...M19.046{Sunday}

The next setup required is to set the desire hour we want the drive to turn on the motor and the hour to turn off.  In this case we just use the hour to setup the hourly schedule. 

**Scheduler 1 hour to start motor** : M18.011 | **Scheduler 1 hour to stop motor** :M18.012

**Scheduler 2 hour to start motor** : M18.013 | **Scheduler 2 hour to stop motor** :M18.014

**Scheduler 3 hour to start motor** : M18.015 | **Scheduler 3 hour to stop motor** :M18.016

The 3 scheduler instances are combined by a logic OR  as we show below .  The OR output is linked to M18.050.

![The 3 Scheduler Instances](https://github.com/luisgcu/MCS-Scheduler/blob/master/docs/Scheduler%20general%20view.jpg)

### Detailed Setup

1- Clone this repository to a location in  your PC, to do that  just click "clone or download"  and the click download  as ZIP. 

![](https://github.com/luisgcu/RTC-Scheduler/blob/master/docs/Download%20repository.jpg)





 










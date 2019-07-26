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










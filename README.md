# Stalker
TouchDesigner 088 and 099 Process Watcher and Manager

## Overview
Most process watchers around work on .exe files. This is a problem for TouchDesigner since project files are .toe files. This small app was designed to be as lightweight as possible and works by creating a minimal amount of communication needed between TouchDesigner and Stalker to determine if your project is alive or dead. If it's dead, Stalker takes full screen screenshots, logs error times and system resource stats, sends an email notification to you with the log attached, and then proceeds to terminate and relaunch the TouchDesigner project.

## Features
* Specifically built to monitor TouchDesigner088 projects
* Uses messaging heartbeats to determine project state
* Receives framerate and process resource usage from TouchDesigner088 process
* Continually forces focus and ensures your TouchDesigner088 project is the top-most window
* Logs error times and system stats
* Takes a full screen screenshot of the crashed system
* Sends an email notification when TouchDesigner crashes and when system is rebooted
* Terminates stalled processes and relaunches them if needed
* Reboots the whole system after repeated process crashes/stalls

## Pre-requisites
Stalker requires Microsoft Visual Studios Redistributables 2015 x86. The installation for this can be found in the root folder as vc_redist.x86.msi.

## Setup
Stalker is self-managing after you set it up. Do NOT run Stalker.exe before finishing all of these setup instructions.
First, we need to define all the settings. Stalker reads all of its settings from a JSON file located inside of the data folder named settings.json. Here is an example settings file and explanation of the values below.

```
{
   "project_details":{
      "path":"C:\\Some_folder\\use\\double\\backslash\\some.toe",
      "name":"Name of your project"
   },
   "email_credentials" : {
      "username" : "YOUR.USERNAME@gmail.com",
      "app_password": "yourGeneratedAppPassword"
   },
   "error_email_settings":{
      "to_email":"RECEIVER@poorSoul.com",
      "subject_line":"The project just died",
      "body":"A friendly note that the project here has died, thanks!"
   },
   "screen_setup":{
      "resolution_x":1920,
      "resolution_y":1080
   },
   "launch_wait_timer_in_seconds" : 30,
   "TCP_port" : 11999,
   "time_till_termination_in_seconds": 60,
   "reboot_flag":true,
   "number_of_crashes_before_reboot":2
}
```

## Project details
In the project_details dictionary, path should be the path to your TouchDesigner project file. Be sure to use double-back slashes in the path. The name key is what you call the project and this name will be used in the email notifications.

## Email credentials
In the email_credentials dictionary, username is your Gmail username. I suggest making a new Gmail account for each installation to avoid Google shutting off your account, as Google doesn't like bot mailing. You will need to enable 2-factor authentication in the Gmail account, and then generate a new App Password to use here (not your login password). Here are the instructions to do that:

1) Go to this link and follow the instructions to setup 2-Step Verification on the new Google account. This will require confirmation via SMS.

2) Go to this link and in the Select an app dropdown, choose Other and enter Stalker

3) In the Select a device dropdown, choose Other and enter the name of your installation.

4) You will be shown the app password, which looks like 4 blocks of 4 characters. Copy and paste this app password into the app_password field. NOTE: It looks like there is a space between each of the 4 blocks, but there isn't. The password should look like chsnirjfoskrhenf.

## Email settings
In the error_email_settings dictionary you can outline the email notification you will receive. The to_email should be the email address that should receive the email. The subject_line and body are respectively the email subject and the body.

## Screen setup
In the screen_setup dictionary you should specify the full Windows resolution as the resolution_x and resolution_y keys. The full Windows resolution should be all your displays, not just the size of your TouchDesigner window.

## Misc settings
The launch_wait_timer_in_seconds is the amount of time Stalker will sleep for after it launches your process (either on initial boot or on crash. This should be a conservative amount of time. I recommend timing how long your project takes to open, and then doubling that.


The TCP_port is the TCP/IP port that Stalker uses to communicate with the Stalker.tox companion component. In general you shouldn't need to change it, but it is available if for some reason you do need to change it. Be sure to also update the TCP/IP port inside the Stalker.tox.


The time_till_termination_in_seconds is the amount of time Stalker will wait after the last received update from TouchDesigner before deciding it is crashed, terminating it, and then reopening it. Again, this should be a conservative amount, some like a minute or two minutes.


The reboot_flag is a bool (true or false) on whether you would like Stalker to reboot the system after a certain number of crashes to the TouchDesigner process.


The number_of_crashes_before_reboot is the amount of process restarts Stalker will attempt before rebooting if the reboot_flag is set to true.


## Usage
After you have finished setting up Stalker, follow these steps: 

1) Start your TouchDesigner project file.

2) Add Stalker.tox anywhere in your project

3) Save and close your project

4) Run Stalker.exe


Stalker is a process watcher and manager, which means that you should never start the project file yourself. Stalker will always take care of running the project, even when you first want to launch the project, you should launch Stalker and it will launch the project for you.


For permanent installations you should create a .bat file or startup action that will run Stalker.exe on boot. Stalker will take care of the rest and boot your project and monitor it going forward.


If you need to close your project or stop process monitoring, close Stalker first, then proceed to close your project.

## JSON validity
Stalker will check the validity of the settings.json file. If there is an issue with it, Stalker will display a JSON error. Use a tool like this JSON Formatter to confirm that your JSON file is valid. Also confirm that the path to the project is correct.

## Logs
Log files are saved in data\logs. It creates a new log file each day to minimize file size. When the system crashes you will receive the log file for that day.

## Screenshots
Screenshots of the system are saved in data\captures. In most cases full size screenshots may be quite large png files, so you will have to retrieve them manually when you access the system. The format of the name is:

`screenshot_YEAR_MONTH_DAY_HOUR_MIN_SECONDS`

## Limitations
* Currently only works with a single TouchDesigner088 process running on the system

## Source
Source files and build instructions coming soon!

## Attribution
* [Boxcutter](https://github.com/mdrasmus/boxcutter)
* [ofxJsonSettings](https://github.com/mattfelsen/ofxJsonSettings)
* [ofxSMTP](https://github.com/bakercp/ofxSMTP)
* [ofxSSLManager](https://github.com/bakercp/ofxSSLManager)

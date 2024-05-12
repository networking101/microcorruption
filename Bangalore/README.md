# Bangalore
## TLDR

## Details
The LockIT Pro c.01  is the first of a new series  of locks. It is
controlled by a  MSP430 microcontroller, and is  the most advanced
MCU-controlled lock available on the  market. The MSP430 is a very
low-power device which allows the LockIT  Pro to run in almost any
environment.

The  LockIT  Pro   contains  a  Bluetooth  chip   allowing  it  to
communiciate with the  LockIT Pro App, allowing the  LockIT Pro to
be inaccessable from the exterior of the building.

There  is no  default  password  on the  LockIT  Pro HSM-2.   Upon
receiving the  LockIT Pro,  a new  password must  be set  by first
connecting the LockitPRO HSM to  output port two, connecting it to
the LockIT Pro App, and entering a new password when prompted, and
then restarting the LockIT Pro using the red button on the back.
    
LockIT Pro Hardware  Security Module 2 stores  the login password,
ensuring users  can not access  the password through  other means.
The LockIT Pro  can send the LockIT Pro HSM-2  a password, and the
HSM will  directly send the  correct unlock message to  the LockIT
Pro Deadbolt  if the password  is correct, otherwise no  action is
taken.

Lockitall engineers  have worked for  over a year to  bring memory
protection to  the MSP430---a  truly amazing achievement.  Each of
the 256  pages can  either be executable  or writeable,  but never
both, finally  bringing to  a close  some of  the issues  in prior
versions.

This  is Hardware  Version  C. It  contains  the all-new  modified
MSP430  with hardware  memory protection.   This hardware  version
also contains the Bluetooth connector  built in, and two available
ports: the LockIT Pro Deadbolt should  be connected to port 1, and
the LockIT Pro HSM-2 should be connected to port 2.

This is Software Revision 01. The new firmware supports the memory
protection we have introduced in this new hardware version.

## Solution
Start at main.

![main](./screenshots/main.png)

This challenge calls a function called set_up_protection before going to login. See what's at this function.

![set_up_protection](./screenshots/set_up_protection.png)

We see multiple calls to mark_page_executable and mark_page_writable before turn_on_dep is called.

![dep](./screenshots/dep.png)

These are some interrupt calls we have not seen before. The [manual](https://github.com/networking101/microcorruption/tree/main/manual.pdf) shows that INT 0x11 will make a page only writable or executable. 

![memory_protection](./screenshots/memory_protection.png)

## Answer
Username: (hex) 25782578  
Password: (hex) 4141414141414141<leak+0x196>00ff\<leak-0xa2>  

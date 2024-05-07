# Algiers
## TLDR

## Details
The LockIT Pro d.01  is the first of a new series  of locks. It is
controlled by a  MSP430 microcontroller, and is  the most advanced
MCU-controlled lock available on the  market. The MSP430 is a very
low-power device which allows the LockIT  Pro to run in almost any
environment.

The  LockIT  Pro   contains  a  Bluetooth  chip   allowing  it  to
communiciate with the  LockIT Pro App, allowing the  LockIT Pro to
be inaccessable from the exterior of the building.

LockIT Pro Account Manager solves the problem of sharing passwords
when  multiple users  must  have  access to  a  lock. The  Account
Manager contains  a mapping of users  to PINs, each of  which is 4
digits.  The  system supports  hundreds of users,  each configured
with his or her own PIN,  without degrading the performance of the
manager.

There are no accounts set up  on the LockIT Pro Account Manager by
default. An administrator must first initialize the lock with user
accounts  and  their  PINs.  User  accounts  are  by  default  not
authorized  for access,  but can  be authorized  by attaching  the
Account  Manager  Authorizer.  This  prevents  users  from  adding
themselves to the lock during its use.
    
This is Hardware  Version D.  It contains  the Bluetooth connector
built in, and one available port, to which the LockIT Pro Deadbolt
should be connected. When authorizing PINs, the Deadbolt should be
disconnected and the Authorizer should be attached in its place.

This   is  Software   Revision   01.  It is a  much more  advanced
version of other locks, but the first Version D release.

## Solution
Start on login.

![login](./screenshots/login.png)

This challenge introduces malloc and free functions. We will probably need to use a heap overflow to overwrite data on free. Looking at the function, each malloc allocates 0x10 bytes but each getsn function reads 0x30 bytes from the user. The data is not written to the stack but we can overwrite heap buffer metadata. Check where free happens on the buffers.

![login2](./screenshots/login2.png)

The second buffer is freed first. This gives us the ability to abuse the second buffer's metadata with the first call the getsn.

I wanted to start by reversing the malloc and free functions. You can find my notes on these functions [here](https://github.com/networking101/microcorruption/blob/main/Addis%20Ababa/heap_functions_reverse.txt).

## Answer
Password: (hex) 

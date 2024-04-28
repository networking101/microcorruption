# Reykjavik
## TLDR
This program is vulnerable to buffer overflow.  
If the byte at address 0x2410 == 0xe7, access will be granted.  
This byte is the 17th character of user input.  

## Details
The LockIT Pro a.03  is the first of a new series  of locks. It is
controlled by a  MSP430 microcontroller, and is  the most advanced
MCU-controlled lock available on the  market. The MSP430 is a very
low-power device which allows the LockIT  Pro to run in almost any
environment.

The  LockIT  Pro   contains  a  Bluetooth  chip   allowing  it  to
communiciate with the  LockIT Pro App, allowing the  LockIT Pro to
be inaccessable from the exterior of the building.

There is  no default password  on the LockIT  Pro---upon receiving
the LockIT Pro, a new password must be set by connecting it to the
LockIT Pro  App and  entering a password  when prompted,  and then
restarting the LockIT Pro using the red button on the back.
    
This is Hardware  Version A.  It contains  the Bluetooth connector
built in, and one available port  to which the LockIT Pro Deadbolt
should be connected.

This is Software Revision 02. This release contains military-grade
encryption so users can be confident that the passwords they enter
can not be read from memory.   We apologize for making it too easy
for the password to be recovered on prior versions.  The engineers
responsible have been sacked.

## Solution
Start on login.

![login](./screenshots/login.png)

The user's input is stored starting at address 0x2400. The I/O Console states that passwords are between 8 and 16 characters but the getsn function will read 0x1c characters.

The test_password_valid function uses interrupt 0x7d to check the password. We have no way of finding the correct password in program memory.

The result of test_password_valid is checked in the login function. If the check failed (returned 0 in r15), we jump to 0x4552 and compare the byte at 0x2410 to 0xe7. If this byte matches, access is granted.

![login2](./screenshots/login2.png)

We can unlock the door if the 17th byte of our password input is 0xe7.

## Answer
Password: (hex) e7e7e7e7e7e7e7e7e7e7e7e7e7e7e7e7e7

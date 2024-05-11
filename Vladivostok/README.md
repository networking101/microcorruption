# Vladivostok
## TLDR
This program uses ASLR to randomize the stack and text sections.  
A format string vulnerability is used to leak an address on the text section.  
A buffer overflow can be used to overwrite the return address of the function at base+0x82.  
Use a ROP gadget to pop 0x00ff into the sr register.  
Use another ROP gadget to call INT.  

## Details
The LockIT Pro c.05  is the first of a new series  of locks. It is
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

Despite the  year of development  effort which  went in to  it, we
have heard  reports that  the memory  protection introduced  in to
LockIT Pro r e.01 is insufficient. We have removed this feature in
favor of  the tried-and-true HSM-2. The  engineers responsible for
LockIT Pro r e.01 have been sacked.
    
This is Hardware  Version C.  It contains  the Bluetooth connector
built in, and two available  ports: the LockIT Pro Deadbolt should
be  connected to  port  1,  and the  LockIT  Pro  HSM-2 should  be
connected to port 2.

This is  Software Revision 05.  We have implemented  new state-of-
the-art techniques to prevent any futher lock issues.

## Solution
Start on main.

![main](./screenshots/main.png)

There is not much here. The rand function is called twice and a memcpy moves a large amount of memory that contains instructions to a random address. Then the function ends by jumping to r13. It looks like this challenge implements ASLR (Adress Space Layout Randomization) to obfuscate the code and to prevent an attacker jumping to a static instruction address.

The program wipes the original instructions but we can view the disassembly before running the program.

![aslr_main](./screenshots/aslr_main.png)

I copied the instructions to a text file so I could make edits...

## Answer
Username: (hex) 25782578
Password: (hex) 4141414141414141<leak+0x196>00ff<leak-0xa2>

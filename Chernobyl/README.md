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


```
new AAAAAAAAAAAAAAAA 0
new AAAAAAAAAAAAAAAA 1
new AAAAAAAAAAAAAAAA 2
new AAAAAAAAAAAAAAAA 3
new AAAAAAAAAAAAAAAA 4
new BBBBBBBBBBBBBBBB 5
6e657720 063e 8010 424242424242424242424242 2030
6e657720 f43d 8010 01 2030
new juju0 0
new juju1 0
new juju2 0
new juju3 0
new juju4 0
6e6577206a756a75 3c50 9c50 4242

464d7f 20 30
```
TODO point back to buffer, not start of heap structure

New hash table: 0x533c - 0x5987 (0x64b bytes)

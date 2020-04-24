## fanuc_io

### OVERVIEW

This is a simple Karel based IO read/edit service with TCP/IP and User Socket Messaging by FANUC, you will need to setup two servers on your Teach pendant
preferably S5:, S6:,

### SETUP SERVERS
ON FANUC TEACH PENDANT DO ; 

> MENU > SETUP > ==> > ==> > Host Comm > F4 > Servers

Here you should be able to see listed servers, The programs rpovoded in the repo are configured to use S5 AND S6.
Configure and start this servers, THE PROTOCOL should be SM(socket messaging)

## CONFIGURE PORTS
ON FANUC TEACH PENDANT DO ; 

> MENU > NEXT > SYSTEM > ==> > Variables > $HOSTS_CFG 

Here you will see lists of avaliables hosts, according to the index of server that you configured in above step(S5 S6 IN THIS CASE);
enter to that index and find $SERVER_PORT to an avaliable port, for the programs in this repo this ports are configured to be 
S5 ; 59002
S6 ; 59003

## TRANSFER .KL TO .PC (BUILD KAREL SOURCE FILES) 

.kl files includes source codes but they need to be built in order them to be transfered to robot controller
for this unfourtunetly you will need a windows machine and Licence from FANUC for ktrans.exe compiler. You also need to be careful to target the correct version of your controller running on. For example I compile the files and target them to V8.30-1 version with;

> "C:\Program Files (x86)\FANUC\WinOLPC\bin\ktrans.exe" write_io_server.kl /ver V8.30-1


> "C:\Program Files (x86)\FANUC\WinOLPC\bin\ktrans.exe" read_io_server.kl /ver V8.30-1

the .pc files corresponding to .kl files should be created under same directory, the repo currently contains .pc files for specified version 
 
Transfer the .pc files to controller via USB or FTP , REFER TO FANUC manual for details

## RUN THE PROGRAMS
if you would like to run these programs concurently, write a TP program with Teach Pendant, it should look like;

RUN IO_SERVER_RD
RUN IO_SERVER_WR

and then start this program with 

> Deadman Switch + SHIFT + RESET

> Deadman Switch + SHIFT + FWD

You can also run each program individually from teach pendant

 ## A QUICK TEST WITH TELNET

 In order to test this works, you can use TELNET(I used windows machine but it is also avaliable for Linux)

FOR IO READ SERVICE, in a windows machine start a command prompt 

> telnet robot_ip 59002 

This should start the IO data flow in the prompt

FOR IO WRITE SERVICE, 
similarly

> telnet robot_ip 59003

Enter 5 digits
where;
- the first digit specifies IO tYPE(1 = DOUT, 2 = RDO, 3 = AOUT)
- the 3 digits in middle specifies IO index(from 1 up tp 512)
- the last digit is IO value to be set

and check wheter that worked


**NOTE**: Do not use this on production systems or in contexts where any kind
of determinism is required.

## Limitations / Known issues
* No check wheter IO write was successfull
* only LIMITED IOs are used
* requires Socket Messaging option to be installedon Fanuc Robot controller

## Bugs, feature requests, etc

Please use the GitHub issue tracker.


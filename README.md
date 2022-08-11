~~~~~~~~~~~~~~~~

//***FILE 123 is from Sam Golob and contains a primitive system     *   FILE 123
//*           to limit the use of TSO commands in a TSO session.    *   FILE 123
//*           Sometimes, you can't give a group of users the full   *   FILE 123
//*           spectrum of TSO commands beyond the ones they really  *   FILE 123
//*           need.  See member $$NOTES for more details.           *   FILE 123
//*                                                                 *   FILE 123
//*           This file contains several programs which reflect     *   FILE 123
//*           the TSO command entered, back to the terminal,        *   FILE 123
//*           without doing any other action.  If you want to       *   FILE 123
//*           block a user from using a particular TSO command,     *   FILE 123
//*           create a load library to concatenate (in a STEPLIB)   *   FILE 123
//*           ahead of all the other libraries that their TSO       *   FILE 123
//*           session has access to, then put one of these three    *   FILE 123
//*           programs into that library.  I greatly prefer to use  *   FILE 123
//*           the ECHOPGM command for this.  Then create an ALIAS   *   FILE 123
//*           member to the ECHOPGM program (using the PDS program  *   FILE 123
//*           from File 182 of the CBT Tape).  When you enter       *   FILE 123
//*           a command with that alias name, the ECHOPGM program   *   FILE 123
//*           is executed, and it reflects that command's name      *   FILE 123
//*           back to the terminal, with all its parameters.        *   FILE 123
//*                                                                 *   FILE 123
//*           If the user enters a "forbidden" program, his/her     *   FILE 123
//*           command is merely reflected back to the terminal,     *   FILE 123
//*           with all the parameters, and it doesn't do anything.  *   FILE 123
//*           Therefore the system is "protected" from that user,   *   FILE 123
//*           and the commands entered also can be logged.          *   FILE 123
//*                                                                 *   FILE 123
//*           Using the PDS command processor on File 182, you      *   FILE 123
//*           can create hundreds or thousands of aliases to one    *   FILE 123
//*           program.  It's usually easiest to do this with        *   FILE 123
//*           PDS running under TSO-in-batch.  If you really want   *   FILE 123
//*           to get fancy, just copy ECHOPGM to a new library,     *   FILE 123
//*           and then you can ALIAS all the program names in       *   FILE 123
//*           SYS1.CMDLIB and the other TSO command libraries.      *   FILE 123
//*           Then you delete the aliases for the commands the      *   FILE 123
//*           users are permitted to use.  Make sure there are      *   FILE 123
//*           enough directory blocks in the STEPLIB library,       *   FILE 123
//*           to contain all the alias names.  On a 3390 pack,      *   FILE 123
//*           45 directory blocks are on a track.  44 are on the    *   FILE 123
//*           first directory track, and 45 on all the others.      *   FILE 123
//*                                                                 *   FILE 123
//*           Here's some sample TSO-in-batch JCL to create         *   FILE 123
//*           aliases to a TSO command program.                     *   FILE 123
//*                                                                 *   FILE 123
//*       //SAGOLOBT  JOB (ACCT#),S-GOLOB,                          *   FILE 123
//*       // NOTIFY=&SYSUID,                                        *   FILE 123
//*       // CLASS=S,MSGCLASS=X                                     *   FILE 123
//*       //*                                                       *   FILE 123
//*       //TSOBATCH EXEC PGM=IKJEFT01                              *   FILE 123
//*       //STEPLIB  DD DISP=SHR,DSN=library.where.PDS.is           *   FILE 123
//*       //SYSTSPRT DD SYSOUT=*                                    *   FILE 123
//*       //SYSTSIN  DD *                                           *   FILE 123
//*        PDS 'your.new.steplib'                                   *   FILE 123
//*        ALIAS ECHOPGM command1                                   *   FILE 123
//*        ALIAS ECHOPGM command2                                   *   FILE 123
//*        ALIAS ECHOPGM command3                                   *   FILE 123
//*        ALIAS ECHOPGM command4                                   *   FILE 123
//*        END                                                      *   FILE 123
//*       /*                                                        *   FILE 123
//*              Do this to as many commands as you want.           *   FILE 123
//*                                                                 *   FILE 123
//*           The three ECHO*** programs included here are:         *   FILE 123
//*                                                                 *   FILE 123
//*        ECHOADF  - Only works under the TSO Session Manager,     *   FILE 123
//*                   EXEC PGM=ADFMDF03 in the logon proc.          *   FILE 123
//*                   (Don't use this program--we have better.)     *   FILE 123
//*                                                                 *   FILE 123
//*        ECHOTPUT - Reflects the entire contents of the command   *   FILE 123
//*                   buffer back to the terminal, using TPUT.      *   FILE 123
//*                   (Don't use this program--we have better.)     *   FILE 123
//*                                                                 *   FILE 123
//*        ECHOPGM  - Reflects the entire contents of the command   *   FILE 123
//*                   buffer back to the terminal, using PUTLINE.   *   FILE 123
//*                   (This program can echo up to 250 characters.  *   FILE 123
//*                    See the MAXMSG label in the EPUTL csect.)    *   FILE 123
//*                   (Use this program. It's the best we have.)    *   FILE 123
//*                                                                 *   FILE 123
//*    ---->   ECHOPGM is the preferred program to use.     <----   *   FILE 123
//*            ------- -- --- --------- ------- -- ---              *   FILE 123
//*    ---->    The other programs are included only as     <----   *   FILE 123
//*    ---->    coding examples and for experimentation.    <----   *   FILE 123
//*                                                                 *   FILE 123
//*          Sam Golob                                              *   FILE 123
//*                                                                 *   FILE 123
//*      email:  sbgolob@cbttape.org  or  sbgolob@attglobal.net     *   FILE 123
//*                                                                 *   FILE 123
~~~~~~~~~~~~~~~~


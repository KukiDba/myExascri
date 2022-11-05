# myExascri
my stuff


================================
'
4'difference between wright-through and write-back 
flashcache mode
protocol used for communication between database server and storage server

EXADATA and SuperCluster : Check if long running transactions are preventing min active scn from progressing, resulting in Storage Indexes not being used (Doc ID 2081483.1)	


ASM parameters specific to X

CELLIP.ORA
 
components of Exadata

Exadata rack options

storage index and how it works
 flash cache and how it w6orks?

What is cellcli?

What is dcli?

Explain ILOM 
Background services of Cell Server
spine switch?

Exadata shutdown and start-up procedure
EHCC?

high capacity and high performance disk?

Flash cache?

Smart Scan?

*******************************

FICO 

***********************************
http://karandba.blogspot.in 
http://rajkumar-dba.blogspot.com/2019/08/useful-exadata-ib-switches-command.html?m=1---------------IB switch command
http://rajkumar-dba.blogspot.com/2018/09/how-to-check-oracle-exadata-model.html?m=1------------To check exa model




*******************

1. How we enable samart scan for obect level.(https://centroid.com/exadata-smart-scan-processing/)
2. After migrating our performance degarde veryf badly in exadata. How do you tune it.
3. What is storage indexes.


******************************************************************
HCL ojt interview 


1.            What are the types of EHCC(exadata Hybrid Columar compression) ?
 
2.            What is flash cache and how it works ?
 
3.            What is the purpose of spine switch ?
 
4.            What is the difference between write-through and write-back flashcache mode ?

https://www.oracle.com/technetwork/articles/database/exadata-write-back-flash-2179184.html
Ans:"write-through" method data is written to the main memory through the cache immediately, 
while in "write-back" data is written in a "latter time"
 
5.            What is the difference between DBRM AND IORM ?
 
6.            How smart scan works ?
 
7.            What are the unique features of Exadata ?
 
8.            Which all networks available in Exadata ?
 
9.            Which processes are used by storage software ?
 
10.           What is smart scan offloading ?
 
11.           What is the primary goal of storage index ?
 
12.           Which tool is used to generate initial configuration files based on customer's data ?(OEDA)


***************************************************************************************************
node evication

http://www.dbas-oracle.com/2013/06/Top-4-Reasons-Node-Reboot-Node-Eviction-in-Real-Application-Cluster-RAC-Environment.html


Grid Disk replacement
**************************
https://docs.oracle.com/en/engineered-systems/exadata-database-machine/sagug/exadata-administering-asm.html#GUID-C3B0ED41-B03E-4EBE-80A2-A0A9338B491B


If a grid disk remains offline longer than the time specified by the disk_repair_time attribute, 
then Oracle ASM force drops that grid disk and starts a rebalance to restore data redundancy.
Oracle ASM monitors the rebalance operation, and Oracle Exadata System Software sends an e-mail message when the operation is complete.




Oracle India 

1. Exadata day to day activity.
2. 

How to Replace a Hard Drive in an Exadata Storage Server (Predictive Failure) (Doc ID 1390836.1)
http://dbasravan.blogspot.com/2017/04/exadata-flash-card-or-damaged-hard.htmssl
12.2 Grid Infrastructure and Database Upgrade steps for Exadata Database Machine running 11.2.0.3 and later on Oracle Linux (Doc ID 2111010.1)
SR 3-19517853171 : CRS upgrade of 12cr1 TO 12CR2 on Exadata
Auto disk management feature in Exadata (Doc ID 1484274.1)
How to Reduce SYSAUX Tablespace Occupancy Due to Fragmented TABLEs and INDEXes (Doc ID 1563921.1)
How to resize ASM disks in Exadata (Doc ID 1684112.1)
How to resize ASM disks in Exadata (Doc ID 1684112.1)



Humancios
*************************

1. Describe few phsical components in exadata.(Infiband,KVM,PDU,spine switch,HCC,IORM,smart flash cache).
2. Which protocal used of infiband.(RDS,zero data protocal)
3. How flash cache improve the perfermance.
4. 1/8 eight and quarter.
5. What is use of ILOM.
6. Which command is used of ILOM ip.
7. What is Cell and grid disk.
8. Quarter rack how many cell disk on per node
9. How do you say 12 hard disk in cell disk.
10.How do you monitor during exadata patching.
11.How do you check status of infiband.(ibswithes,ibhost).
12.how to check entire cpu or core in entire system.(Enabled or disbled core or socket)



Flash disk replacment procedure:
*************************************

Steps to shut down or reboot an Exadata storage cell without affecting ASM (Doc ID 1188080.1)


Steps to power down or reboot a cell without affecting ASM:
When performing maintenance on Exadata Cells, it may be necessary to power down or reboot the cell.
If a storage server is to be shut down when one or more databases are running, 
then verify that taking the storage server offline will not impact Oracle ASM disk group and database availability. 
The ability to take Oracle Exadata Storage Server offline without affecting database availability depends on the level of Oracle ASM redundancy used on the affected disk groups, and the current status of disks in other Oracle Exadata Storage Servers that have mirror copies of data as Oracle Exadata Storage Server to be taken offline.
=====================================================================================================================================================

1)  By default, ASM drops a disk shortly after it is taken offline; however, you can set the DISK_REPAIR_TIME attribute to prevent this operation by specifying a time interval to repair the disk and bring it back online. The default DISK_REPAIR_TIME attribute value of 3.6h should be adequate for most environments.

(a) To check repair times for all mounted disk groups - log into the ASM instance and perform the following query: 
SQL> select dg.name,a.value from v$asm_diskgroup
dg, v$asm_attribute a where dg.group_number=a.group_number and
a.name='disk_repair_time';

(b) If you need to offline the ASM disks for more than the default time of 3.6 hours then adjust the parameter by issuing the command below as an example:
SQL> ALTER DISKGROUP DATA SET ATTRIBUTE 'DISK_REPAIR_TIME'='8.5H';
    Note : As of version 12.1.0.1, there is a new diskgroup attribute, failgroup_repair_time, 
that governs the time a cell is allowed to be offline before its disks are dropped. 
This value defaults to 24 hours thus obviating the need to alter the disk_repair_time attribute as in older versions.

2)  Next you will need to check if ASM will be OK if the grid disks go OFFLINE. The following command should return 'Yes' for the grid disks being listed:
cellcli -e list griddisk attributes name,asmmodestatus,asmdeactivationoutcome

3) If one or more disks does not return asmdeactivationoutcome='Yes', you should check the respective diskgroup and restore the data redundancy for that diskgroup.
    Once the diskgroup data redundancy is fully restored, repeat the previous command (step #2).
    Once all disks return asmdeactivationoutcome='Yes', you can proceed with taking the griddisk offline (step #4).

Note: Shutting down the cell services when one or more grid disks does not return asmdeactivationoutcome='Yes' will cause Oracle ASM to dismount the affected disk group, causing the databases to shut down abruptly.
 
4) Run cellcli command to Inactivate all grid disks on the cell you wish to power down/reboot:
cellcli -e alter griddisk all inactive

* Please note - This action could take 10 minutes or longer depending on activity. It is very important to make sure you were able to offline all the disks successfully before shutting down the cell services. Inactivating the grid disks will automatically OFFLINE the disks in the ASM instance. 

5) Confirm that the griddisks are now offline by performing the following actions:
(a) Execute the command below and the output should show either asmmodestatus=OFFLINE or asmmodestatus=UNUSED and asmdeactivationoutcome=Yes for all griddisks once the disks are offline in ASM. Only then is it safe to proceed with shutting down or restarting the cell:
cellcli -e list griddisk attributes name,asmmodestatus,asmdeactivationoutcome
( there has also been a reported case of asmmodestatus= OFFLINE: Means Oracle ASM has taken this grid disk offline. This status is also fine and can proceed with remaining instructions)

(b) List the griddisks to confirm all now show inactive:
cellcli -e list griddisk

Stop the cell services using the following command:

CellCLI> ALTER CELL SHUTDOWN SERVICES ALL



6) You can now reboot the cell. Oracle Exadata Storage Servers are powered off and rebooted using the Linux shutdown command.
(a) The following command will shut down Oracle Exadata Storage Server immediately: (as root):
#shutdown -h now
(When powering off Oracle Exadata Storage Servers, all storage services are automatically stopped.)

(b) The following command will reboot Oracle Exadata Storage Server immediately and force fsck on reboot:
#shutdown -F -r now

7) Once the cell comes back online - you will need to reactive the griddisks:

Start the cell services using the following command:

CellCLI> ALTER CELL Start SERVICES ALL
cellcli -e alter griddisk all active

8) Issue the command below and all disks should show 'active':
cellcli -e list griddisk

9)  Verify grid disk status:
(a) Verify all grid disks have been successfully put online using the following command:
cellcli -e list griddisk attributes name, asmmodestatus

(b) Wait until asmmodestatus is ONLINE for all grid disks. Each disk will go to a 'SYNCING' state first then 'ONLINE'. The following is an example of the output:
DATA_CD_00_dm01cel01 ONLINE
DATA_CD_01_dm01cel01 SYNCING
DATA_CD_02_dm01cel01 OFFLINE
DATA_CD_03_dm01cel01 OFFLINE
DATA_CD_04_dm01cel01 OFFLINE
DATA_CD_05_dm01cel01 OFFLINE
DATA_CD_06_dm01cel01 OFFLINE
DATA_CD_07_dm01cel01 OFFLINE
DATA_CD_08_dm01cel01 OFFLINE
DATA_CD_09_dm01cel01 OFFLINE
DATA_CD_10_dm01cel01 OFFLINE
DATA_CD_11_dm01cel01 OFFLINE
(c) Oracle ASM synchronization is only complete when all grid disks show asmmodestatus=ONLINE.
     ASM rebalance phase will start automatically after disk resync phase completes if using GI version 12.1 or higher.
( Please note:  this operation uses Fast Mirror Resync operation - which does not trigger an ASM rebalance if using GI version < 12.1. The Resync operation restores only the extents that would have been written while the disk was offline.)
  

10) Before taking another storage server offline, Oracle ASM synchronization must complete on the restarted Oracle Exadata Storage Server. If synchronization is not complete, then the check performed on another storage server will fail. The following is an example of the output:

CellCLI> list griddisk attributes name where asmdeactivationoutcome != 'Yes'
DATA_CD_00_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_01_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_02_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_03_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_04_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_05_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_06_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_07_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_08_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_09_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_10_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"
DATA_CD_11_dm01cel02 "Cannot de-activate due to other offline disks in the diskgroup"








 Replace a Hard Drive in an Exadata Storage Server
********************************************************


1. Check the Disk having issue.

 
CellCLI> LIST PHYSICALDISK WHERE diskType=HardDisk AND status LIKE ".*failure.*" DETAIL
         name:                   20:3
         deviceId:               21
         diskType:               HardDisk
         enclosureDeviceId:      20
         errMediaCount:          14
         errOtherCount:          0
         luns:                   0_3
         makeModel:              "SEAGATE ST32000SSSUN2.0T"
         physicalFirmware:       061A
         physicalInsertTime:     2014-06-30T14:15:33+00:00
         physicalInterface:      sas
         physicalSerial:         L2KDFT
         physicalSize:           1863.0166854858398G
         slotNumber:             3
         status:                 warning - predictive failure

CellCLI>


2. Check the diskgroups..

 
SQL> 1
  1* select group_number,name,state from v$asm_diskgroup
SQL> /

GROUP_NUMBER NAME                                               STATE
------------ -------------------------------------------------- ---------------------------------
           1 DATA_EXA2                                          MOUNTED
           2 ARCH_EXA2                                          MOUNTED
           3 RECO_EXA2                                          MOUNTED
           4 DATA2_EXA2                                         MOUNTED
           5 RECO2_EXA2                                         MOUNTED
           6 DATA1_CORP                                         MOUNTED
           7 DATA2_CORP                                         MOUNTED
           8 DATA3_CORP                                         MOUNTED
           9 DATA4_CORP                                         MOUNTED
          10 DATA5_CORP                                         MOUNTED
          11 DATA6_CORP                                         MOUNTED
          12 DATA7_CORP                                         MOUNTED
          13 DATA8_CORP                                         MOUNTED
          14 DATA9_CORP                                         MOUNTED
          15 DATA10_CORP                                        MOUNTED
          16 DATA11_CORP                                        MOUNTED
          17 DATA12_CORP                                        MOUNTED

17 rows selected.

SQL>
3.Check if any rebalance operation running.

 
SQL> select * from gv$asm_operation where state='RUN';

no rows selected

SQL>
4. check the status of disk to be replaced.

 
CellCLI> list celldisk where lun=0_3
         CD_03_exa2cel06         proactive failure

CellCLI> list griddisk where celldisk=CD_03_exa2cel06 attributes name,size,status,asmmodestatus
         ARCH_EXA2_CD_03_exa2cel06       29.109375G      proactive failure       DROPPED
         DATA4_CORP_CD_03_exa2cel06      300G            proactive failure       DROPPED
         DATA_EXA2_CD_03_exa2cel06       1532.546875G    proactive failure       DROPPED

CellCLI>
Optional but i always do it.

 
CellCLI> alter physicaldisk 20:3 drop for replacement
Physical disk 20:3 was dropped for replacement.

CellCLI>
 
CellCLI> list physicaldisk 20:3 detail
         name:                   20:3
         deviceId:               21
         diskType:               HardDisk
         enclosureDeviceId:      20
         errMediaCount:          14
         errOtherCount:          0
         makeModel:              "SEAGATE ST32000SSSUN2.0T"
         physicalFirmware:       061A
         physicalInsertTime:     2014-06-30T14:15:33+00:00
         physicalInterface:      sas
         physicalSerial:         L2KDFT
         physicalSize:           1863.0166854858398G
         slotNumber:             3
         status:                 warning - predictive failure - dropped for replacement

CellCLI>
5.Now inform Oracle support to replace the Disk. Once done do the validation.

 
CellCLI> list physicaldisk 20:3 detail
         name:                   20:3
         deviceId:               22
         diskType:               HardDisk
         enclosureDeviceId:      20
         errMediaCount:          0
         errOtherCount:          0
         luns:                   0_3
         makeModel:              "SEAGATE ST32000SSSUN2.0T"
         physicalFirmware:       061A
         physicalInsertTime:     2014-10-24T14:33:23+00:00
         physicalInterface:      sas
         physicalSerial:         L89VNM
         physicalSize:           1863.0166854858398G
         slotNumber:             3
         status:                 normal

CellCLI>
 
CellCLI> list celldisk where lun=0_3 detail
         name:                   CD_03_exa2cel06
         comment:
         creationTime:           2014-10-24T14:33:23+00:00
         deviceName:             /dev/sdd
         devicePartition:        /dev/sdd
         diskType:               HardDisk
         errorCount:             0
         freeSpace:              0
         id:                     4948604a-ee85-47c3-b644-740531516331


         interleaving:           none
         lun:                    0_3
         physicalDisk:           L89VNM
         raidLevel:              0
         size:                   1861.703125G
         status:                 normal

CellCLI>
 
CellCLI> list griddisk where celldisk=CD_03_exa2cel06 attributes name,size,status,asmmodestatus
         ARCH_EXA2_CD_03_exa2cel06       29.109375G      active  ONLINE
         DATA4_CORP_CD_03_exa2cel06      300G            active  ONLINE
         DATA_EXA2_CD_03_exa2cel06       1532.546875G    active  ONLINE

CellCLI>






*************************************************************
CAPGMINI (EXADATA)

1. In EHCC define package which give us compresision ration(DBMS_COMPRESSION).
https://ittutorial.org/hcc-exadata-hybrid-columnar-compression-in-oracle-exadata-1/

https://uhesse.com/2011/09/12/dbms_compression-example/

http://nayakvishwanath.blogspot.com/2016/09/why-hcc-hybrid-columnar-compression-hcc.html



2. What is IDP in ASM(https://hemora.blogspot.com/2010/08/asm-intelligent-data-placement.html)

Starting 11gr2,

ASM now classifies reads/writes into two - COLD and HOT. As you guessed, HOT read/writes are those coming from the blocks which are classified as HOT - meaning placed in the outer sectors. COLD is the default - meaning data placed in inner sectors. Data blocks of most often accessed segments are candidates for placement in HOT region. This is done at the datafile level in an ASM disk.

3. In expdp which parameter give us elasped time of each object(LOGIN).
4. What is advanced statics.
5. 











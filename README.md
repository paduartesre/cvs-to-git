# CVS-to-GIT

----------- Pre-requisites before to initial process conversion -------------

1. Backup full of CVS repository
2. Upload or copy file backup to machine below
3. Machine with reccommendations:
   - Recommended 8 Core processor
   - Recommended 24 GB Memory RAM
   - 1 Disk SSD M2.NVME or UltraSSD provide for Microsoft Azure (Used this scenario UltraSSD Disk provide by VM Azure)
   - 1 Disk only for backup other files
   - Install python, git, cvs and cvs2svn

------------------------------------------------------------------------------------------------------------

Process conversion of repository CVS to GIT


---------------------------- First step to migration CVS to GIT ---------------------------------------------

1º step - Python Configuration

Install python in your distro.

<b>sudo apt-get update <br>
sudo apt-get install python3.6</b>

Before start process conversion, open the file python below and insert parameters about threads processing also procedure have performed in repository CVS with more than 12 years data and much more than five hundred thousand commits for processing.

**`vim /usr/lib/python2.7/dist-packages/cvs2svn_lib/changeset.py`**

import sys<br>
sys.setrecursionlimit(50000)  #value is 50000 or above

------------------------------------------------------------------------------------------------------------

2º step - Perfom conversion full repository full CVS to SVN in a dump file.

In your distro Linux, execute this command:

**`cvs2svn  --encoding=ascii --encoding=utf8 --encoding=utf16 --encoding=latin --dumpfile=/mnt/n/cvs2/project-repo.dump --trunk=project-repo/trunk --branches=project-repo/tags  /mnt/n/cvs2/Backup-CVSStorage/Data/CVS/Repository/project-repo`**

In my case, this process duration of 12 hours.

------------------------------------------------------------------------------------------------------------

3º step (VM Windows) - Import dump file in VisualSVN to review and validate data of repository in the dump. To import file dump VM Linux to Windows, use SAMBA, or a tool of transfer files, FTP, SFTP or any service of cloud transfer files between VMs. In this case I perform the transfer, configuring a machine Linux in a server file of service MEGA with RSA4096 bits. The procedure to accomplish this, its found here in my repo portfolio.

Install the VisualSVN, open the program and select in the top menu -> **`New -> Import Repository.`**

After this, select the file dump and wait for the import process to finish.


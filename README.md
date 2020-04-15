# cvst-to-git

----------- Pre-requisites before to initial process conversion -------------

1. Backup full of CVS repository
2. Upload or copy file backup to machine below
3. Machine with reccommendations:
   - Recommended 8 Core processor
   - Recommended 24 GB Memory RAM
   - 1 Disk SSD M2.NVME or UltraSSD provide for Microsoft Azure (Used this scenario UltraSSD Disk provide by VM Azure)
   - 1 Disk only for backup other files

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


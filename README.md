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

------------------------------------------------------------------------------------------------------------

4° step - Execute GIT-SVN to convert SVN to GIT

In the servidor where is installed and configured SVN, GIT, perform command below to generate file authors.txt. This command have perform inside repository SVN in the Linux.

<b>1º option of command:</b> **`svn log | grep '^r[0-9]' | awk '{print $3}' | sort | uniq `**

or 

<b>2º option of command:</b> **`svn log -q /opt/svn/svn-win/trunk | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > /home/path/your_folder/authors.txt`**

or

<b>3º option of command:</b> **`svn log -q https://url-your-svnrepo/svn/project-repo/trunk | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > /home/path/your_folder/authors.txt`**

--
This get list, the display of items that confirmed, filter lines that initial with a revision, number, author, date, list and delete duplicates lines.
 ( r[12345] | author | date-and-stuff...), print (autor), list and delete duplicates lines. )

------------------------------------------------------------------------------------------------------------

5º passo - Executar a migração para o GIT sem o arquivo authors.txt

git svn clone https://ti-0199.clsoftware.com.br/svn/projects2/ "C:\git\projects2" -s --no-metadata

O comando poderá solicitar a credencial de acesso ao SVN que foi configurado no momento da instalação do VisualSVN.

Executar a migração para o GIT com o arquivo authors.txt

git svn clone https://ti-0199.clsoftware.com.br/svn/projects2/ "C:\git\projects2" -s --no-metadata --authors-file="authors.txt"

--------
Linux

git svn clone -r1:HEAD --no-minimize-url --stdlayout --no-metadata --authors-file /home/pedro/authors/authors.txt https://ti-0199.clsoftware.com.br/svn/projects2/

------------------------------------------------------------------------------------------------------------

6º passo - Migrar tags SVN para formato tags GIT


git for-each-ref refs/remotes/tags | cut -d / -f 4- | grep -v @ | while read tagname; do git tag "$tagname" "tags/$tagname"; git branch -r -d "tags/$tagname"; done

------------------------------------------------------------------------------------------------------------



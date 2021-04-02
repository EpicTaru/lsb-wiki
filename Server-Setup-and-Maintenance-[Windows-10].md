##### Table of Contents  
- [Install](#install)  
- [Update](#update)  

# Install

## 1. Software needed:

### ⚠️ Make sure you have the latest version of each software mentioned on this page installed on your system.

To build, run, and maintain a Topaz server, you will need to download and install the following third-party software:

**All of:**
* [Git for Windows](https://gitforwindows.org/): Accept defaults.
* [Visual Studio Community 2019](https://visualstudio.microsoft.com/vs/community/): Under "Workloads" > check "Desktop development with C++". Creating/linking a free account may be required to use it. Used to compile and build the source files.
* [MariaDB](https://mariadb.org/): Includes HeidiSQL GUI frontend for viewing and editing your SQL database, just remember the password you set for MariaDB during the installation. Used as SQL database. _**New versions of MySQL from Oracle will NOT work. Use MariaDB!** MySQL hasn't worked properly out of the box since 5.7 - that's a long time ago!_
* [Python 3](https://www.python.org/downloads/): **Download the latest version!** Check "Add Python 3.x to PATH". Click on "Customize installation" then everything should be checked on the first window that shows up > Next. Options 2, 3 and 4 should be checked on the second window. Install > Close. Make sure .py files are associated with Python by default so you can double click on them to open them.

**Optional:**
* [GitHub Desktop](https://desktop.github.com/), a Git GUI for managing git repositories and branches.
* [TortoiseGit](https://tortoisegit.org/), a Git GUI for managing git repositories and branches.

## 2. Download the source code:
<details>
  <summary>Git for Windows</summary>
  
1. Open a PowerShell window and navigate to your chosen install directory.
2. Type:
```
git clone --recursive https://github.com/DerpyProjectGroup/topaz.git
```
</details>
<details>
  <summary>GitHub Desktop</summary>
  
1. Open GitHub desktop. File > Clone repository > URL button (along top).
2. Enter the following:
  * Repository URL: either [your forked copy of our repository](https://raw.githubusercontent.com/wiki/DerpyProjectGroup/topaz/images/github-fork.png) `https://github.com/your-github-name/topaz.git` (recommended), or our repository `https://github.com/DerpyProjectGroup/topaz.git`
  * Local path: Where you want the source code to live on your computer.
3. Select `Clone` button:
[[/images/github-desktop-clone.png|Pull Origin button location]]
</details>
<details>
  <summary>TortoiseGit</summary>
  
1. Right click wherever you want to download the repository > Git Clone... > URL: https://github.com/DerpyProjectGroup/topaz.git ("release" branch by default) > OK > then Close when it's done. 

2. Don't forget about Navmeshes (https://github.com/topaz-next/xiNavmeshes.git): right click on the freshly downloaded "topaz" folder > TortoiseGit > Submodule Update... > OK > then Close when it's done.
</details>

## 3. .conf files configuration:

In the `topaz\conf\default\` folder, make sure you **copy** all the files in there and put them in the precedent folder (`topaz\conf\`) like the readme.md file says. Open these three files (with the default Windows text editor (Notepad) or an external one (like  [Notepad++](https://notepad-plus-plus.org/)):

* login.conf
* map.conf
* search_server.conf

then modify this line each time:

> mysql_password: root 

(replace "root" with your MariaDB password)

## 4. Preparing the database:

In the `topaz\tools\` folder, right click + holding the Shift key > context menu: Open command window here/Open PowerShell window here.: then type:

```
py -3 -m pip install -r requirements.txt
```
It will download and install MySQL Connector Python, gitpython, pyyaml, colorama and pip.

Followed by:
```
py -3 dbtool.py
```
(or just double click on `topaz\tools\dbtool.py`)

In the new dbtool window:

"Database tpzdb (default name, change it in the .conf files if you want a different one) does not exist. Would you like to create new database: tpzdb? [y/N]" > y > Enter

Done.

## 5. Build the servers:

**We have a new build system, read how to use it here:**

https://github.com/DerpyProjectGroup/topaz/wiki/CMake-Build-Guide

* topaz_connect.exe
* topaz_game.exe
* topaz_search.exe

should be present in the `topaz\` folder.

Servers are now configured properly. Execute all the above .exes (as Administrator, you can create a shortcut for each one and topaz_search.exe is optional, if you want to use the search function and pydarkstar).

**Next Step: [Post-Install Guide](https://github.com/DerpyProjectGroup/topaz/wiki/Post-Install-Guide)**

# Update

First, pull updated server files from Topaz Next.
<details>
  <summary>Git for Windows</summary>
  
1. In the `topaz\` folder, open a PowerShell window.
2. Type:
```
git stash
git pull
git stash pop
```
</details>
<details>
  <summary>GitHub Desktop</summary>
  
1. Open GitHub Desktop. Next to where your current branch is listed, click either `Fetch origin` (checking for updates), or `Pull origin`
[[/images/pull_origin.png|Pull Origin button location]]
</details>
<details>
  <summary>TortoiseGit</summary>
  
1. Right click wherever you want > TortoiseGit > Settings > Context Menu > check: "Pull..." > Apply > OK.

2. Right click on the "topaz" folder > Git Pull... > Remote Branch: (select or type) "release" (stable) or "canary" (master) > OK > Close.
</details>

After pulling the new files:

* RERUN EVERY .sql MODIFIED FILES: Open a PowerShell window in the `topaz\tools\` folder and type:
```
py -3 dbtool.py update
```
This will import all of the .sql files that were updated and run any needed migrations. It will also possibly overwrite any custom changes you have made to your SQL tables. If you are running custom mobs/items/etc. of any kind, you'll want to save these as queries in a .sql file in `topaz\sql\backups\` and use the Restore/Import option in dbtool to import those changes after an update.

❔ For more information on database management, see [Database Management](https://github.com/DerpyProjectGroup/topaz/wiki/Database-Management) or [Preparing the Database](https://github.com/DerpyProjectGroup/topaz/wiki/Server-Setup-and-Maintenance-%5BWindows-10%5D/#4-preparing-the-database).

* REBUILD THE SOLUTION IF ANY .cpp/.h/.in IS MODIFIED (referring to the whole example at **[5. Build the servers](https://github.com/DerpyProjectGroup/topaz/wiki/Server-Setup-and-Maintenance-%5BWindows-10%5D/#5-build-the-servers)**).
* RESTART YOUR SERVER(S) FOR .conf FILES AND AFTER UPDATES WITH dbtool.
* .lua files ARE INSTANT IN MOST CASES (`topaz\scripts\globals\` .luas will need to be reloaded by using the GM command `!reloadglobal` where appropriate or restarting the server).
* ⚠️ In the `topaz\conf\default\` folder, make sure you take any .conf file that was updated and put it/them in the precedent folder (`topaz\conf\`). Do not overwrite version.conf, as dbtool uses it to track DB version and will update CLIENT_VER for you if it changes. ⚠️
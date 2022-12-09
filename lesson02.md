# Lesson 2: 
Build the Software Development Environment
For our software development environment we will need the following components:
- Node.js (back-end framework - runs on server)
- Vue.js (front-end HTML/Javascript framework - runs on client)
- Quasar (front-end CSS framework - runs on client)
- MySQL (relational database - runs on server)
- VSCode (code editor) 
- Git (version control)



### <a name="install-git"></a>Install Git
On Google search "install git without brew"
click on the entry "Install Git | Atlassian Git Tutorial"
Scroll down the screen to "Download the latest Git for Mac Installer". Click the link
Click on the link for "Download Latest Version" (green button)
Open the window with the downloaded package and double click. A message will appear that the file cannot be installed
Open the Apple Settings -> Security & Privacy. On the bottom is a message and button that allows you to install the package.
Install the Git package
Open the Terminal and enter:
```
git --version
```
If Git was installed you will see the version message.

Create a Git configuration for yourself:
```
git config  - - global user.name “Henning Seip”
git config  - - global user.email “Henning.Seip@candogram.com”
```

### <a name="install-node"></a>Install Node.js
On Google search for "Mac download node.js"
Click on the entry called "macOS Installer" to download the package.
Install the package by clicking on the downloaded file.
Test the installation:
Open a terminal and enter:
```
node
```
You will see a welcome message. Use Crtl+D to exit Node.js


### <a name="install-vue"></a>Install Vue.js and Quasar
Goto to http://quasar.dev
The Quasar installation include the Vue.js package. Vue.js can be installed without Quasar and used with other CSS frameworks or none.
Click the "Getting Started" link, then Quasar CLI.
From the page use the following command and enter it into the Terminal, perceded by the superuser "sudo"
```
sudo npm i -g @quasar/cli
```

### <a name="install-vscode"></a>Install VSCode editor
On Google search for "download VSCode"
On the Microsoft page click the download for Mac. A ZIP archive is downloaded
Click on the downloaded ZIP archive to extract the app. 
Drag the app into the Application folder
Open the VSCode editor and install the following packages:
- Volar
- Prettier
- ESLint
- Auto Rename Tag
- Rest Client

Use the Command palette (CMD+Shift+P) to add support to start VSCode from the terminal
In the Command palette enter "shell command". Click on the entry "Install 'code' command in PATH"

Click File -> Auto Save to turn on automatically saving of all edits. 



### <a name="install-mysql"></a>Install MySQL and MySQL Workbench
Preparation:
There are many version of MySQL Server. For the correct version you need to find out you MacBook's OS version and chip type:
- Click the Apple -> About This Mac
- On the screen note the main Version (ie. 11) and the Chip (ie. Apple M1)

Install the database:
On Google search for "download mysql"
Scroll to MySQL Downloads, click on MySQL Community Server
On the MySQL Downloads screen, click on the Archives tab
Select the latest product version that matches your MacBook's OS version. A list of downloadable packages will be displayed.
From the list of downloadable packages find the one that matches your MacBooks OS version and Chip. For M1 it is ARM, DMG Archive.
Click the Download button.
Once the package has downloaded, doubleclick the package and walk through the install process.
IMPORTANT: Choose
- "Use Legacy Password Encryption"
- use "basebase" as the password.

After the install:
Open the Mac Terminal to configure the MySQL database:
- Set the path to find MySQL
  ```
  sudo nano /etc/paths
  ```
  add the following line to the end of the file:
  ```
  /usr/local/mysql/bin
  ```
- Add a configuration file:
  ```
  sudo nano /etc/my.cnf
  ```
  add the following lines:
  ```
  [mysqld]
  secure_file_priv=''
  local-infile=1
  ```

- Add a user
```
mysql -u root -p
```
Enter the password you created during the MySQL install process.
In MySQL enter:
```
create user 'baseuser'@'localhost' identified by 'base';

grant all privileges on *.* to 'baseuser'@'localhost';

flush privileges;

exit;
```

Activate the MySQL configuration file:
In the Systems Settings click on the MySQL app
Click "Stop MySQL Server"
Click the Configuration tab
Click Configuration File box, add the path and filename:
```
/etc/my.cnf
```
Click the Apply button
Restart the server on the Instance tab and close the app

- Install MySQL Workbench
On Google search for "MacOS MySQL Workbench" and go to the Download page
Download and install the MySQL Workbench
Open the Workbench from the Mac Launchpad
Click on the wrench and 
- change the name of the default connection to "BASE"
- change the Username to 'baseuser'
Hi the Test Connection button and enter the password 'base' for the 'baseuser'. With the connection valid go the next step.

Click the Advanced tab and add the following entry to the Others box:
```
OPT_LOCAL_INFILE=1
```
Hit the Close button.
Click on the Connection to open the connection SQL window. Create a database using the following SQL command:
```
create database BASE;
```
Click the flash button to execute the SQL command. 
Place a # sign in front of the SQL command to comment it out

Download the data file with the job titles:  
https://candogram-downloads.s3.amazonaws.com/BASEJobInterest.csv.zip  

Unzip the file with the Finder.

In the MySQL Workbench window enter the following SQL to create the tables and load the data:
```
use BASE;

CREATE TABLE `Student_Job_Interest` (
  `User` char(60) NOT NULL Comment'hashed name for privacy',
  `School` varchar(128) NOT NULL,
  `JobTitle` varchar(128) NOT NULL,
  `CreatorEmail` varchar(64) not null Comment'student who created the polling software',
  PRIMARY KEY (`User`,`School`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

CREATE TABLE `Jobs_In_Demand` (
  `JobTitleID` int NOT NULL,
  `JobTitle` varchar(128) NOT NULL,
  `EmployerCount` int NOT NULL,
  `JobCount` int NOT NULL,
  `AvgAnnualWage` int NOT NULL,
  `RequiredEducation` varchar(32) NOT NULL,  
  `City` varchar(32) NOT NULL,
  `StateCode` char(2) NOT NULL,
  `TrackingMonth` int NOT NULL,
  `TrackingYear` int NOT NULL,
  PRIMARY KEY (`JobTitleID`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

load data local infile '/Users/henningseip/Downloads/BASEJobInterest.csv' into table Jobs_In_Demand fields terminated by ',' optionally enclosed by '"' lines terminated by '\n' IGNORE 1 LINES;
```







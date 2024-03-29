# Lesson 2: 
Build the Software Development Environment
For our software development environment we will need the following components:
- Node.js (back-end framework - runs on server)
- Vue.js (front-end HTML/Javascript framework - runs on client)
- Quasar (front-end CSS framework - runs on client)
- MySQL (relational database - runs on server)
- VSCode (code editor) 
- Git (version control)

### <a name="install-brew"></a>Install Brew
We install the MacOs package manager Brew. Open Chrome and goto the web page https://brew.sh/ 
Next open a terminal on your Macbook. For this click the Launchpad and click the Other tools icon. Here you find the Terminal. Click on Terminal to start it. From the Brew homepage (Chrome) copy the shell command and paste it into your terminal. Hit enter. This will install Brew. Once the installation has finished you need to add Homebrew to your PATH. Copy / paste the following commands into your terminal and hit enter:
```
    echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/student/.zprofile
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/student/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
```

### <a name="install-git"></a>Install Git
We now install Git. Git is a software version control system. In your terminal enter:
```
brew install git
```
Then check the Git installation. In your terminal enter:
```
git --version
```
If Git was installed you will see the version message.

Create a Git configuration for yourself (use your name and BASE email address):
```
git config  --global user.name “Henning Seip”
git config  --global user.email “Henning@bronxsoftware.org”
git config  --global init.defaultBranch main
```

### <a name="install-node"></a>Install Node.js
On Google search for "**Mac download node.js**"
Click on the entry called "**macOS Installer**" to download the package.
Install the package by clicking on the downloaded file.
Test the installation:
Open a terminal and enter:
```
node
```
You will see a welcome message. Use Crtl+D to exit Node.js


### <a name="install-vue"></a>Install Vue.js and Quasar
The Quasar installation include the Vue.js package. Vue.js can be installed without Quasar and used with other CSS frameworks or none. The documenation is here: Goto to http://quasar.dev  
Enter the install command into the Terminal, proceeded by the superuser "**sudo**"
```
sudo npm i -g @quasar/cli
```

### <a name="install-vscode"></a>Install VSCode editor
On Google search for "download VSCode"
On the Microsoft page click the download for Mac. A ZIP archive is downloaded.
Click on the downloaded ZIP archive to extract the app. 
Drag the app into the Application folder.
Using Launchpad open the VSCode editor and install the following packages:
- Volar
- Prettier
- ESLint
- Auto Rename Tag
- Rest Client

Use the Command palette (**CMD+Shift+P**) to add support to start VSCode from the terminal
In the Command palette enter "**shell command**". Click on the entry "Install 'code' command in PATH"

Click File -> Auto Save to turn on automatically saving of all edits.  
Close VSCode.  


### <a name="install-mysql"></a>Install MySQL and MySQL Workbench
Preparation:
There are many version of MySQL Server. For the correct version you need to find out you MacBook's OS version and chip type:
- Click the Apple -> About This Mac
- On the screen note the main Version (ie. 11) and the Chip (ie. Apple M1)

Install the database:
On Google search for "**download mysql**"
Scroll to MySQL Downloads, click on **MySQL Community Server**
On the MySQL Downloads screen, click on the **Archives** tab
Select the latest product version that matches your MacBook's OS version. For Mac v11 the latest version is **8.0.28**. A list of downloadable packages will be displayed.
From the list of downloadable packages find the one that matches your MacBooks OS version and Chip. For M1 it is ARM, DMG Archive.
Click the Download button.
Once the package has downloaded, doubleclick the package and walk through the install process.  
**IMPORTANT:** Choose
- "Use Legacy Password Encryption"
- use "**baseroot**" as the password.

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
Save the file and close the Nano editor with Ctrl+X and 'Y' + ENTER

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
  Save the file and close the Nano editor with Ctrl+X and 'Y' + ENTER
  Close the Terminal by selecting "**Quit Terminal**" in the menu.  
  Re-open the Terminal to activate the new **paths** setting.

Now we add a user to MySQL:
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
Click "**Stop MySQL Server**" .  
Click the Configuration tab 
Click the check box "Configuration File".  
Click Configuration File box, add the path and filename:
```
/etc/my.cnf
```
Click the Apply button.
Restart the server on the Instance tab and close the settings app.

Now we install **MySQL Workbench**.  
On Google search for "**MacOS MySQL Workbench**" and go to the Download page.
CLick the Download button. Ignore the Login and Sign up buttons. Click the link below "**No thanks, just start my download**"
Download and install the MySQL Workbench by dragging it to the Applications icon.  
Open the Workbench from the Mac Launchpad. If you are prompted to install **Rosetta** then do so.
Click on the wrench to open the configuration.  
- Select the default connection "**Local instance 3306**"
- Change the name of the default connection to "BASE"
- change the Username to 'baseuser'
Hit the Test Connection button and enter the password '**base**' for the '**baseuser**'. With the connection valid go the next step.

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

load data local infile '/Users/student/Downloads/BASEJobInterest.csv' into table Jobs_In_Demand fields terminated by ',' optionally enclosed by '"' lines terminated by '\n' IGNORE 1 LINES;
```







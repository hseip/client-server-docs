# Lesson 3:
Build the Student Poll app.  
Open the Terminal. It will default to your Home directory.
Create a new directory called "BASE" and cd into it
```
mkdir BASE
```
Use Finder -> Go -> Home to locate the BASE directory. Drag the BASE directory to the left side bar-> Favorites for easy access.

Using the Terminal change the current directory to BASE:

```
cd BASE
```

In the BASE directory create a new directory called student-poll:
```
mkdir student-poll
```
change the current directory to student-poll:
```
cd student-poll
```

Prepare this directory for software version control with Git:
```
git init
```
Next we create a subdirectory and an empty file in preparation for our production server on AWS. We will need it once we are ready to move our project to production.  
```
mkdir AWS_install_scripts
cd AWS_install_scripts
touch appspec.yml
cd ..
```
Make sure that you are back at the **student-poll** directory level!   


Next initialize a new app called 'client' using Vue.js/Quasar:
```
npm init quasar
```

Follow the prompts. Keep hitting enter except for:
- Project Folder: client
- Quasar App: BASE Student Poll
- Pick Quasar App CLI variant: Quasar with Vite

Next create a new directory in student-poll called 'server', change your current directory and initialize it with NPM:
```
mkdir server
cd server
npm init -y
```
Change the directory to the level above (student-poll) and start the VSCode editor (don't forget to add the dot after 'code' with a blank space in between):
```
cd ..
code .
```
Close the Terminal.

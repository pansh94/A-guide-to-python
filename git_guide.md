# GIT

## Configuration
To see the configuration info. **.gitconfig** is your configutaion file but we use cmd command to change and retrieve that.
```
git help # tp get help page
cat .gitignore  # to read the file
git config --global color.ui true # to enable the color interface
git config --global core.editor "name" #to change the default editor for git
git config --list  # to list the settings 
```
1. System :
```
git config --system user.name/user.email "value"
```
2. User
```
git config --global user.name/user.email "value"
```
3. Project
```
git config user.name/user.email "value"
```

## Getting started
Initialize a git project to tell git that start tracking the change set. If you rmeove .git dir it will stop git form tracking
our project. You can only touch config in .git to alter the project level settings, everything else you will leave alone.
```
git init #to initialize git repo
ls -la .git #to check inside git repo(dont mess with these dir) 
# after making changes, commit the chnages
git add . # add all changed file s to git repo
git commit -m "message" # commit the changes to repo
```
### Writing commit messages
A commit message should be able to describe the changes you are making in the commit set.
1. It should be short single line summary message(50 char).
2. Optionally followed by blank line and more description message(for large changes in the commit set, even then keep every 
line less than 72 char).
3. Write commit message in present tense not in past tense.
4. for bullet points addd * or -.
5. Use label like feature/bug fix/tracking no.(#num)
6. Be clear and descriptive.
ex 
```
#380 - Fixes bug in admin logout.
LIttle bit description what you did
Problem:solution pair should be there
```




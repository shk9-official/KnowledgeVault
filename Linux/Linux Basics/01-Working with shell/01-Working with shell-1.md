

[Linux-Basics-Course-Study-Guide-1-1.pdf](Attachments/01-Working%20with%20shell-1-20250211.pdf)

### Introduction to Shell

**Home Directory** 
- Location -> `/home/user_name`
- Only the logged in user can access the home directory
- Represented by "~"
- `echo $HOME` shows home directory of current user

**Command and Argument**
- $ echo Hello
- $ echo -n Hello
- $ command {options} {argument}
	- `echo` -> command
	- `-n` -> option
	- `Hello` -> argument


![01-Working with shell-1-20250211.png](Attachments/01-Working%20with%20shell-1-20250211.png)


**Command types**
1. Internal commands
	1. These are build-in commands ~30 odd.
	2. Ex: `echo`, `cd`, `pwd`, `mkdir`
2. External commands
	1. From external packages, binaries, scripts
	2. Ex: `mv`, `uptime`
$ `type uptime` , $ `type echo`, $ `type mv`

![01-Working with shell-1-20250211-1.png](Attachments/01-Working%20with%20shell-1-20250211-1.png)




---

### Basic linux commands


- `pwd` -> Present working directory
- `ls` -> List contents of directory
- `mkdir` -> Creates new directory
	- Ex: `mkdir Asia Europe America Africa`
- `cd Asia` -> Changes directory to Asia
- `mkdir -p Asia/India/Mumbai` -> Creates all directories in the path, if not present
- `cd ..` -> Moves up one directory
- `cd` -> Changes present working directory to home directory
- Absolute path -> Location of file or directory from`/`
- Relative path -> Location of file or directory from present working directory
- `mv src dest` 
	- Moves file/directory from source to destination
	- Source and destination can be both absolute path or relative path
	- `mv` can also be use to rename files and folders

![01-Working with shell-1-20250211-2.png](Attachments/01-Working%20with%20shell-1-20250211-2.png)

![01-Working with shell-1-20250211-3.png](Attachments/01-Working%20with%20shell-1-20250211-3.png)

- `pushd` -> Remembers current working directory before changing directory
- `popd` -> Last pushed directory comes out first

![01-Working with shell-1-20250211-4.png](Attachments/01-Working%20with%20shell-1-20250211-4.png)

- `cp src dest` -> Copy file/directory from source to destination
- `cp -r src dest` -> Recursive copy

`rm {file_name}` -> Deletes file/directory. Use with `-r` option for recursive

- `cat file_name` -> Concatenate command. Shows contents of file
- `cat > file_name` 
	- Input content to file
	- `ctrl+d` to end
	- Will replace existing contents

`touch file_name` -> Creates a file

![01-Working with shell-1-20250211-5.png](Attachments/01-Working%20with%20shell-1-20250211-5.png)

`ls`
- List Storage
- Lists contents of directory
- `ls -l`
	- Long list
	- Provides more information about files and folders inside directory
- `ls -a`
	- Lists all file including hidden files (Preceded by `.`)
- `.` -> Current working directory
- `..` -> Directory one level up
- `ls -lt`
	- Lists files in order it was created
	- Oldest on top
- `ls -ltr`
	- Lists files in reverse order created
	- Newest on top

![01-Working with shell-1-20250211-6.png](Attachments/01-Working%20with%20shell-1-20250211-6.png)

#### Pagers

`more file_name`
- Displays file content in a scrollable way - Loads entire file in memory
- {Space} -> Scrolls one screen full of data at a time
- {Enter} -> Scrolls the display one line at a time
- {b} -> Scrolls the display backwards one screen full of data at a time
- {/} -> Search text
- {q} -> Quit

`less file_name`
- Displays file content in a scrollable way
- {Up Arrow} -> Scrolls up the display one line
- {Down Arrow} -> Scrolls down the display one line
- {/} -> Search text

---

### Command line help
`whatis {command}`
- One line description of what the command does
- Ex: `whatis date`

- `man {command}` -> Provides entire documentation of the command
- `command --help` -> Provides option and format on how the command should be used
- `apropos {keyword}` -> Searches all man pages for the keyword

---

### Bash shell

Shell types
1. Bourne shell (sh) -> `/bin/sh`
2. C shell (csh or tcsh)
3. Korn shell (ksh)
4. Zshell (zsh)
5. Bourne again shell (bash) -> `/bin/bash`

`echo $SHELL` -> Will show which shell is in use

`chsh`
- Will change shell
- `sudo chsh -s /bin/sh user_name`

![01-Working with shell-1-20250211-7.png](Attachments/01-Working%20with%20shell-1-20250211-7.png)

![01-Working with shell-1-20250211-8.png](Attachments/01-Working%20with%20shell-1-20250211-8.png)

Bash shell features
- Bash auto-completion -> Press `tab`
- Set Alias
	- `alias dt=date` -> `dt` will run `date` command
- Command history
	- `history` -> Will show all previously run commands


#### Bash environment variables
- `$SHELL` -> Environment variable to store the type of shell
- `$HOME` -> Stores home directory path of current user
- `env`
	- Lists all set environment variables
	- Print each using `echo $var_name`
- `export OFFICE=home_office`
	- Set an environment variable using export command
	- Just setting `OFFICE=home_office`, without `export` command, will limit the variable to current shell session
		- Will not be available or carry forwarded to any other processes

To persist across reboots and logins, add to `~/.profile` or ~/.pam_environment in users home directory

![01-Working with shell-1-20250211-9.png](Attachments/01-Working%20with%20shell-1-20250211-9.png)

![01-Working with shell-1-20250211-10.png](Attachments/01-Working%20with%20shell-1-20250211-10.png)

![01-Working with shell-1-20250211-11.png](Attachments/01-Working%20with%20shell-1-20250211-11.png)

PATH variable
- When an external command is used, OS searches for the command (binary) in $PATH
- `echo $PATH`

![01-Working with shell-1-20250211-12.png](Attachments/01-Working%20with%20shell-1-20250211-12.png)

`which` command gives location of program
 - `which obs-studio`

Add to PATH variable
- `export PATH=$PATH:/new/path`
- Ex: `export PATH=$PATH:/opt/obs/bin`

#### Bash Prompt

Bash prompt is customisable
- `PS1` variable defines the bash prompt
- `echp $PS1` -> Shows the current setting of bash prompt
- `PS1 = "[\d \t \u@\h:\w]$"`
	- `\d` - Date
	- `\t` - Time 24 hours
	- `\u` - Current user
	- `\h` - Hostname
	- `\w` - Present working directory
	- `\H` - Complete hostname
	- `\n` - Newline
	- `\r` - Carriage return
	- `\T` - Time 12 hours
	- `\s` - Name of shell
	- `\A` - 24 hour format
	- `\W` - Base name of current working directory
	- `\$` - # if UID is 0, else $
- Append to  `~/.profile` to persist variables (bash prompt) across logins
- `echo 'alias ll="ls -l"' >> ~/.profile` 
	- Adding alias to ~/.profile to persist across logins
- `echo 'PROJECT="Mercury"' >> ~/.profile`
	- Adding an environment variable to ~/.profile to persist across logins
- `echo 'PS1="[\d]\u@\h:\w$"' >> ~/.profile`
	- Change the bash prompt and add to ~/.profile to persist across logins
	- `source ~/.profile` -> Reloads user profile

![01-Working with shell-1-20250211-13.png](Attachments/01-Working%20with%20shell-1-20250211-13.png)

Whenever a user logins,
- ~/.profile. ~/.bash_profile, and ~/.bashrc get executed and sets up personal preferences
- Variables, aliases and configs can be added to these profile files
- `echo 'MY_VAR="my_val"' >> ~/.profile`
	- Adding an environment variable to ~/.profile to persist across logins

---


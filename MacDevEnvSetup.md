##Common development environment setup in Mac

###Bash Shell
Note: OS X doesn't read `~/.bashrc` file on bash start.Instead, it reads the following files (in the following order):
    - /etc/profile
    - ~/.bash_profile
    - ~/.bash_login
    - ~/.profile (Thus put stuff that you typically put in your linux bashrc file in ~/.profile)


1. Custom bash prompt:Set the PS1 variable with needed information shortcuts to set the current prompts settings
   - I have set it to show username and working directory: `PS1='\u:\w\$'`
   - Another common example is to display date/time, user, hostname and current directory. `PS1="[\d \t \u@\h:\w ] $`
   - Ref: http://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html

###Jdk
 Download latest jdk dmg from oracle site and install it.
 >> Confirm using $ java -version

###Maven
 Download latest archive from maven.apache.org. Extract it and put to desired location (typically /usr/local/). Add the bin folder to your PATH environment variable.
###Git
Download and install using git installer dmg file available on sourceforge.

####Configuration

**Where** 

1. System level: System level settings. Usual location is /etc/gitconfig and can be direclty opened by `$ git config --system --edit`. These are usually not altered.
2. User level (i.e all projects for a given system user) git configurations are stored in ~/.gitconfig file. Directly open it by  `$git config --global --edit` and view them by --view option instead of --edit. 
3. Per repo config is stored in pathToLocalGitRepository/.git/config

**How**

1. Directly update these files. See sample in ../gitconfig.txt
2. Use the command line 
   - `$git config [--global| --system] property value`
   - Ex `$ git config --global user.name "Harshit Aggarwal"`


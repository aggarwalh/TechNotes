##Common development environment setup in Mac

###Bash Shell
Note: OS X doesn't read `~/.bashrc` file on bash start.Instead, it reads the following files (in the following order):
    - /etc/profile
    - ~/.bash_profile
    - ~/.bash_login
    - ~/.profile (Thus put stuff that you typically put in your linux bashrc file in ~/.profile)


1. Custom bash prompt:Set the PS1 variable with needed information shortcuts to set the current prompts settings
   - I have set it to show username and working directory: `PS1='\u:\w\$'`
   - Another common example is to display date/time, user, hostname and current directory.
        - `PS1="[\d \t \u@\h:\w ] $`
   - Ref: http://www.cyberciti.biz/tips/howto-linux-unix-bash-shell-setup-prompt.html

###Jdk
 Download latest jdk dmg from oracle site and install it.
 >> Confirm using $ java -version

###Maven
#### Installation
1. Download latest archive from maven.apache.org. 
2. Extract it and put to desired location (typically /usr/local/). 
3. Add the bin folder to your PATH environment variable
4. Test installation using `$ mvn --version`

####Configuration (settings.xml)
**Where** 

1. Global settings at mavenInstallLocation/conf/settings.xml
2. A user level settings: userHomeFolder/.m2/settings.xml
   - In case user's settings.xml doesn't exist you can simply copy global settings file to user's settings location and override as needed.
3. Project level settings can be set in projects pom.xml file
   
**What**

1. A very common use case is to change to default remote mirror to download atrifacts from.
2. Other common configurations are localRepository, offline, servers, mirrors, proxies, profiles, activeProfiles.
3. Ref: https://maven.apache.org/settings.html

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


### Installation
Install from dmg available in site. Configure password for default user postgres

### Post installation setup

Enable command line access:
  -  Export postgres executables location in bash. I added these to bash profile
  -  `export PATH=$PATH:/Library/PostgreSQL/<version>/bin`
  
Server start stop from daemon tools 
  ```
  $ sudo launchctl start com.edb.launchd.postgresql-9.6 
  $ ps -ef | grep postgres ## check if all processes are up
  $ sudo launchctl stop com.edb.launchd.postgresql-9.6
  ```
  
 ### Basic commands
 
 - `$ psql -U postgres` Login to postgres shell
 - `# \q` Exit shell 

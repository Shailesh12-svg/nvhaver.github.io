# Reset a Windows password using Linux

The chntpw command allows this reset. It prefers root privileges to avoid permission issues.
Question: is this command available in Debian based Linux?

1. Mount the Windows partition: mount /dev/sdb2 /mnt/Microsoft/
2. cd to the config folder: cd /mnt/Microsoft/Windows/System32/config/
3. Check Security Account Manager (SAM) database: chntpw -l SAM
4. Edit SAM database: chntpw -i SAM
5. Select edit user data and password (1)
6. Type in user account name
7. Type 1 to clear password, type 2 to set a new password
 

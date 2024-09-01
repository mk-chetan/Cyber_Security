
### Website for help:

- Below website helps to understand the linux command we type.
	- `https://explainshell.com`

### Random Notes:

-  Temp folder has all R,W and X permissions for all 3 groups (Owner, Group and All) , for any malware execution, we can use temp folder to upload and execute the file in the machine
	- Temp path - `/tmp`
- /etc/passwd - Holds the user and their passwords but with a placeholder as x, as below
	- mysterious:x:1001:1001
- /etc/shadow - This file holds the encrypted passwd of the users in the place of the x placeholder
- /etc/sudoers - This file holds the sudoers details and the permissions provided to the users
- /etc/group - This file holds all the different groups including the sudo group, were we can edit and add new users to the sudo group directly or give permissions/access in the /etc/sudoers file
- `ls -la` output, the permissions are displayed as a series of nine characters. The first character represents the file type (e.g., `-` for a regular file, `d` for a directory). The next three characters represent the owner's permissions, followed by the group's permissions, and then the permissions for other users.

	- For example, let's consider an `ls -la` output line:

		-rwxr-x--- 1 user group 4096 May 10 12:34 myfile.txt 

	- In this example, the permissions are broken down as follows:

	- `-rwxr-x---`: The first character indicates that it is a regular file. The following three characters (`rwx`) represent the owner's permissions (read, write, and execute). The next three characters (`r-x`) represent the group's permissions (read and execute). The last three characters (`---`) represent the permissions for other users (no permissions).
	- `1`: Indicates the number of hard links to the file.
	- `user`: Refers to the owner of the file.
	- `group`: Refers to the group assigned to the file.
	- `4096`: Indicates the file size in bytes.
	- `May 10 12:34`: Specifies the date and time of the last modification.
	- `myfile.txt`: Represents the name of the file.


### Useful Commands:

`pwd` (Print Working Directory):

	-  Explanation: Displays the current working directory.
    - Example: Running `pwd` in the terminal would show the absolute path of the current directory, such as "/home/user/documents".

`cd` (Change Directory):

	-  Explanation: Allows you to change the current working directory.
    - Example: Running `cd /home/user/documents` would change the directory to "/home/user/documents".

`cd ..` (Change to Parent Directory):

	- Explanation: Moves up one level in the directory hierarchy.
    - Example: Running `cd ..` in "/home/user/documents" would move to the "/home/user" directory.

`ls` (List Directory Contents):

	- Explanation: Lists the files and directories in the current directory.
    - Example: Running `ls` would display the files and directories in the current directory.

`ls -la` (List Detailed Directory Contents):

	- Explanation: Lists detailed information about files and directories, including hidden files.
    - Example: Running `ls -la` would display a detailed list of files and directories, including hidden files, in the current directory.

`mkdir` (Make Directory):

	- Explanation: Creates a new directory.
    - Example: Running `mkdir new_folder` would create a new directory named "new_folder" in the current directory.

`rmdir` (Remove Directory):

	- Explanation: Removes an empty directory.
    - Example: Running `rmdir empty_folder` would remove the directory named "empty_folder" if it is empty.

`man` (Manual):

	- Explanation: Displays the manual pages for a specified command.
    - Example: Running `man ls` would show the manual pages with detailed information about the `ls` command.

`echo`:

	- Explanation: Displays text or variables as output.
    - Example: Running `echo "Hello, world!"` would output "Hello, world!" in the terminal.

`>` (Output Redirection):

	- Explanation: Redirects the output of a command to a file and overwrites the file if it already exists.
    - Example: Running `echo "Hello" > greeting.txt` would write the text "Hello" to a file named "greeting.txt" or overwrite the file if it exists.

`>>` (Append Output):

	- Explanation: Redirects the output of a command and appends it to a file.
    - Example: Running `echo "World!" >> greeting.txt` would append the text "World!" to the end of the "greeting.txt" file.

`rm` (Remove):

	- Explanation: Deletes files or directories.
    - Example: Running `rm file.txt` would delete the file named "file.txt" from the current directory.

`mv` (Move):

	- Explanation: Moves or renames files and directories.
    - Example: Running `mv file.txt new_directory/file_renamed.txt` would move the file "file.txt" to the "new_directory" and rename it as "file_renamed.txt".

`cp` (Copy):

	- Explanation: Copies files and directories.
    - Example: Running `cp file.txt backup/file_copy.txt` would create a copy of "file.txt" named "file_copy.txt" in the "backup" directory.

`locate`:

	- Explanation: Searches for files and directories in a prebuilt database.
    - Example: Running `locate myfile.txt` would search for the file named "myfile.txt" in the prebuilt database and display its path if found.

`updatedb`:

	-  Explanation: Updates the database used by the `locate` command to reflect recent changes in the file system.
    - Example: Running `updatedb` would update the database, allowing the `locate` command to provide up-to-date search results.

`passwd`:

	- Explanation: Allows a user to change their password. 
	- Example: Running `passwd` would prompt the user to enter their current password and then set a new password.

`chmod` (Change Mode):

	- Explanation: Changes the permissions of a file or directory.
    - Example: Running `chmod +x script.sh` would add the execute permission to the file "script.sh", allowing it to be executed as a script.

`adduser`:

	- Explanation: Creates a new user account.
    - Example: Running `adduser john` would create a new user account with the username "john" and prompt for additional user information.

`su` (Switch User):

	- Explanation: Allows a user to switch to another user account.
    - Example: Running `su jane` would switch to the user account "jane" after entering the password for that account.

`/etc/sudoers`:

	- Explanation: Displays the content of the "/etc/sudoers" file, which contains configuration information for the `sudo` command.
    - Example: Running `/etc/sudoers` would display the configuration directives for `sudo` access and permissions.

`sudo -l`:

	- Explanation: Lists the commands a user is allowed to run with `sudo` privileges.
    - Example: Running `sudo -l` would display the commands and permissions available to the current user with `sudo` access.



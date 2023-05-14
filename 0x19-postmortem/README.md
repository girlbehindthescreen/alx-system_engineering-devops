SERVER ERROR:

This file contains a brief description of an issue that occurred in web-stack debugging projects.  
it was given to me a docker container that was wrongly configured to be broken, I tracked and found the main causes and finally restore the failing services.
Issue Summary:
The server and codes was running but while attempting to launch on other servers, the server failed to connect, i get root identification errors and host errors.
Timeline
from 00:00 till date at multiple times of attempt
March 15:
- 10:00 The service failure was detected
- 11:48 The collateral cause was found
- 12:20 The Root cause was found
- 12:36 The Root and collateral causes was fixed
Root cause
On May 20 at 10:00 with a curl request was detected a wrong response from the server running at the docker container, using the tools given in the Web stack debugging project resources as strace, and tmux, the tracking was started.
at 11:48 I figured out that MySQL wasn’t given any answer, so I checked the service status and Mysql wasn’t running, but I saw this just as a collateral issue rather than the main cause.
at 12:20 the main issue was found while checking the strace a little higher, I noticed that a stat() system call was trying to get a non-existent file with the extension ‘.phpp’, knowing this typo failure I checked the files to track this call and the reference was found in the Wordpress configuration file ‘wp-settings.php’ it was requiring a file named ‘class-wp-locations.phpp’ note the ‘.phpp’, this typo was blocking the process and breaking the right server flow.
Resolution and recovery
At 12:36 was necessary to modify the ‘wp-settings.php’, changing the extension for the required file ‘class-wp-locations.phpp’ in the directory wp-includes from ‘.phpp’ to ‘php’ and save the file, also restart MySQL and apache2 services, then the server was tested with the browser and curl requests, in all cases the route returns a success HTML response and the tracked issue was closed
Corrective and preventative measures
At 13:15 the first version of a repair script was uploaded, it was a dirty solution but it does the work, the script just copy the file from class-wp-locations.phpp to class-wp-locations.php and restart the MySQL service, the next step was checking a way to make a clean fix to the wp-settings file and just replace any wrong extension ‘.php*’ to the right version. this solution could help as a preventative action, it must be preceded by a constant typo and running services analysis before taking it to production.
 


# Directory / File Permissions

icacls <directory> --> displays the ACL for directory, showing the permissions
granted or denied to different users and groups.

icacls <directory> /remove:d "NT AUTHORITY\SYSTEM" --> removed a role being denied to view the
dir


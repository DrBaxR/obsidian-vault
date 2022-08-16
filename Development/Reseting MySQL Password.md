---
tags:
 - db
 - troubleshooting
---
# Resetting MySQL Password
This note describes how you can troubleshoot installation of a [[MySQL]] [[Database]].

Create a file (`C:\mysql-init.txt`) with this in it:
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```
After that, run:
```sh
.\mysqld.exe --init-file=C:\\mysql-init.txt --console
```
This will start the server with the new password for root. To test that the password works, run following command with new password:
```sh
.\mysql.exe -u root -p
```
Delete the file you initially created (`C:\mysql-init.txt`) and stop the server. Next time you start the server it should work just fine with the newly set password.

### Resources
[https://dev.mysql.com/doc/mysql-windows-excerpt/8.0/en/resetting-permissions-windows.html](https://dev.mysql.com/doc/mysql-windows-excerpt/8.0/en/resetting-permissions-windows.html)
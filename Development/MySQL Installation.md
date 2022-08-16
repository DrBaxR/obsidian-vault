---
tags:
 - db
 - backend
---
# MySQL Installation
This note describes the process of installing [[MySQL]] locally.

Get mysql community edition ([https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)) you will get a .zip
Unzip wherever
Open admin terminal in folder where unzipped

```sh
cd bin
./mysqld --install
./mysqld --initialize
```

Now it will appear in [[Windows service]]s (WIN+R > services.msc > MySQL)
It says that it starts on computer boot-up, but you can manually start/stop it here. In order to test the connection to the database, you can run the following command:

```sh
.\mysql.exe -u root -p
```

After that you will be prompted to input a password (it should be empty by default - **no password**). In case password does not work, check [this](https://dev.mysql.com/doc/mysql-windows-excerpt/8.0/en/resetting-permissions-windows.html), or read [[Reseting MySQL Password]].

### Resources
[https://dev.mysql.com/doc/mysql-windows-excerpt/8.0/en/windows-start-service.html](https://dev.mysql.com/doc/mysql-windows-excerpt/8.0/en/windows-start-service.html) (initial setup) 1
[https://stackoverflow.com/questions/35670755/the-mysql-service-on-local-computer-started-and-then-stopped](https://stackoverflow.com/questions/35670755/the-mysql-service-on-local-computer-started-and-then-stopped) (in case service starts and stops instantly after) 2
# Installing MySQL on macOS

1) Get the installation file.
Direct link:
https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.24-macos11-x86_64.dmg

2) Install it
Open the .dmg file you downloaded and install MySQL server Community edition

You might be asked to supply a "password for the mysql root account".
Supply a password that you can remember.

That should be it.

3) Check that mysql is running.
- Open Activity Monitor (press CMD + space, write activity, open "Activity Monitor")
- In the search field in the top right, search for mysql.
If a process is running with the name "mysqld", all is good.
n
![]({{ site.url }}/mysql/install_mac/images/install_mysql_mac_1.png)




4) Ensure mysql will autostart on restart of your computer.

Go to "System Preferences".
Click the "MySQL" icon.

![]({{ site.url }}/mysql/install_mac/images/install_mysql_mac_2.png)

Ensure the checkbox for "Start MySQL when your computer starts up" is checked.

![]({{ site.url }}/mysql/install_mac/images/install_mysql_mac_3.png)


# Installing MySQL Workbench on macOS
1) Get the installation file.
Direct link:

https://dev.mysql.com/get/Downloads/MySQLGUITools/mysql-workbench-community-8.0.24-macos-x86_64.dmg

2) Install it
Open the .dmg file you downloaded, drag the MySQL workbench icon into the Applications folder.

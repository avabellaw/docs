# Creating a LAMP environment on linux to emulate XAMPP

## Install PHP

[Follow this guide](https://computingforgeeks.com/how-to-install-lamp-stack-on-fedora/)

## Install MySQL 

### Directly from Oracle repo

[Follow this guide to install and for post setup](https://docs.fedoraproject.org/en-US/quick-docs/installing-mysql-mariadb/)

*Can uninstall from this link too*

### Fedora community packaged

```bash
sudo dnf install {community-mysql-server|mariadb-server}
```

## Install phpmyadmin

```bash 
sudo dnf install phpmyadmin
```

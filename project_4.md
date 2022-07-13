Start a new ec2 instance to boot with a Ubuntu Server in AWS.
![alt text](/images/01.png)


Let's make sure Ubuntu is Updated and Upgraded to the latest.
```bash
sudo apt update && sudo apt upgrade
```
![alt text](/images/1.png)

Add required certificates
```bash
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
Reading package lists... Done/setup_12.x | sudo -E bash -
```
![alt text](/images/2.png)

Install NodeJS on the server.
```bash
sudo apt install -y nodejs
```
![alt text](/images/02.png)

Next, Install MongoDB Community edition

To do this, when to first import the Import MongoDB GPG public key.
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
![alt text](/images/3.png)

Install MongoDB Server

```bash
sudo apt install -y mongodb
```
![alt text](/images/03.png)

Start Mongodb Server and verify
```bash
sudo service mongodb start
```
```bash
sudo systemctl status mongodb
```
![alt text](/images/4.png)

Install Node package manger ***npm***
```bash
sudo apt install -y npm
```
![alt text](/images/04.png)
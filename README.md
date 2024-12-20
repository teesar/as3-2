# 2420 Assignment 3 Part 2

# Building Two Identical Web/File Servers With A Load Balancer On Digital Ocean.
#### Note:
Please check out my youtube video which shows the installation process and gives an indepth explanation of the files in this repository!
https://www.youtube.com/watch?v=p6aee-binz4


## Requirements:
* The setup script must be run as root or with sudo.
* Digital Ocean Account.
* Arch Linux Image Uploaded to Digital Ocean.

#### Step 1)
* Create two arch linux droplets on digital ocean with a shared tag. I suggest the word "web".
* Move this github repository onto each of your droplets using your preferred method. (github, sftp, etc)

#### Step 2)
Run the provided setup script with sudo or as root on each droplet.
```
sudo ./setup
```

#### Step 3) 
Copy the ip addresses for your droplets from the digital ocean project page, and navigate to them in your web browser toconfirm your servers are up and running.
##### Note: You may add "/documents" after the ip address to view the file server.

#### Step 4)
* Click the Green "Create" rectangle at the top of the digital ocean website.
* Click "Load Balancers" on the menu that opens.
* Selection Regional Load Balancer type.
* Select the datacenter region closest to your geographic area.
* Select External Network Visibility.
* Enter a simple name for your load balancer in the name field such as "Loader"
* Click "Create Load Balancer"

#### Step 5)
* Copy the ip address for the load balancer from the digital ocean project page, and navigate to it through the url bar on your web browser. 
* Confirm your servers are both up and running by repeatedly refreshing the page and watching the displayed "Public IP address of server". This should change between two different numbers as you refresh, indicating both servers are available throught the load balancer.
* Adjust the url bar by adding "/documents" after the ip address. Refresh the page and you will see an index of documents. 
* Left click the file name to download the sample document.
* Congratulations! Youv'e done it!

## What The Setup Script Does:
#### 1) Sets up an nginx web server displaying a static html page containing:

- Kernel Release
- Operating System
- Date
- Number of Installed Packages
- Public IP address of server

Note: This html page updates the first time the script is run, and then every day at 5 am.


#### 2) Configures a ufw firewall that only allows http and ssh traffic (ssh rate limiting enabled).

#### 3) Configures a web file server through nginx which is accessible through adding "/documents" to your load balancer's ip address and navigating to it in your web browser.


## What The Initiation Script Does

#### The initiation script contains some commands I find useful when starting a new server. It syncs, refreshes, and updates system packages and the keyring, installs neovim for a text editor, installs git for github access, sets the global git config user email address to my personal email address, and adds ssh-agent commands to my .bashrc file so I can always connect to ssh through my shell. If you wish to use it, you'll need to substitute my personal email address for yours.

## General Notes From Development Process

* The web server will be set up on port 80, the firewall will allow all outgoing traffic but deny all incoming except for ssh (limited) and http traffic. If these don't suit your purposes you will need to edit the server block in "system-info.conf" to switch the server port, and you may edit the ufw firewall commands in "the_one_script", or continue reading below for manual commands.

* The timer that changes how often the index.html file is refreshed is in generate-index.timer, the current OnCalendar setting means it will run every day at 5am. This may be modified to your desired update frequency.
The OnCalendar format is:
```
* *-*-* *:*:*
```
Where the first portion signifies day of the week, second portion signifies year/month/date, and third portion signifies hours/minutes/seconds.
You may leave out or use any combination of the 3 elements.
To set a command to run every Friday at 5am:
```
OnCalendar=Fri *-*-* 05:00:00
```

* A useful command to both start and enable a service:
```
sudo systemctl enable --now <service-file-name-here>
```

* You can check the status of your firewall with:
```
sudo ufw status verbose
```

* To manually allow another port or service through firewall:
```
sudo ufw allow <port-or-service-here>
```

* To remove a port or service from firewall access:
```
sudo ufw delete allow <port-or-service-here>
```

* If you edit your nginx configuration file you can validate it was written correctly with:
```
sudo nginx -t
```

* When writing a script that moves a file into a new location and then attempts to run it, make sure to order the commands in your script properly to avoid wasting time wondering why the file isn't being run.

* PERMISSIONS PERMISSIONS PERMISSIONS

* Have a nice weekend :)

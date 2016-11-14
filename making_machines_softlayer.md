
FINDMINE Making and Configuring New Machines 
================================

THE SOFTLAYER EDITION
----------------------------------------------------------------

In order to make a FINDMINE machine on softlayer, you will need a login account on [Softlayer][softlayer]

### Step 1: Order Device on Softlayer

1. Log in to [Softlayer][softlayer]
2. Select the ``` Devices ``` tab at the top left 
3. Select the ```Device List``` option
4. Click on the ```Order Devices``` link at the top right
5. Configure the following settings - all other settings leave as default

> Data Center: WDC01
> Computing Instance: 4 x 2.0 GHz Cores
> RAM: 8GB
> Operating System: Ubuntu Linux 16.04 LTS Xenial Xerus Minimal Install (64 bit)
> First Disk: 25GB
> Second Disk: 250 GB

6. Select Add to Cart
7. Configure the Backend VLAN Subnet to be 10.108.117
8. Configure the Frontend VLAN Subnet by picking an available slot
9. Configure the SSH keys by selecting a key from the list. Select the option ```Only for root user```
10. Name your machine according to our conventions 

   	> Example: dev2.wdc       findmine.com

11. Submit Order

### Step 2: SSH Keys
Once your order has been confirmed, create a shell script to log into your new machine using the same configuration as the other login scripts. 

Don't forget to change the IP address to be the private IP address of the new machine.

### Step 3: Configure VPN Client
Add the new machine to your VPN configuration. Use the new machine's private IP address and the netmask 255.255.255.255

### Step 4: Set up new machine as Root
 Log into your new machine using the command

```shell
sudo ssh ./path/to/key root@127.0.0.1
```
 *Substitute the machine's IP address for localhost *

You may see a warning that something bad could be happening (a man in the middle attack etc). 
Use the command ```ssh-keygen``` to manage the keys, by typing:
```shell
sudo ssh-keygen
```
Then try to log in to your new machine again.

```shell
sudo ssh ./path/to/key root@127.0.0.1
```

Once you're in, create the necessary user accounts
```shell
sudo adduser USERNAME
```
Enter the user's name and UNIX password. Then, copy the folder .ssh into the user's directory.
```shell
sudo cp -r .ssh/ /home/USERNAME
```
Then, go into the user's directory and make them the owner of the ssh folder. Make the authorized key file read only.
```shell
cd /home/USERNAME/
chown -R USERNAME:USERNAME .ssh/
cd .ssh/
chmod 400 authorized_keys
```
If necessary, give your user sudo permissions, by typing:
```shell
usermod -a -G sudo USERNAME
```
Confirm that this command was successful with the command
```shell
groups USERNAME
```
You should see ```USERNAME``` and ```sudo``` listed.

If you like, configure bash as you desire.

###Step 5: Prevent Root User from Connecting Remotely

Once you have created a superuser account, you should **login as a superuser** and prevent root from being able to access the machine remotely.  Once you're logged in, edit the ```sshd_config``` file.

Open the file by typing:
```shell
sudo vim /etc/ssh/sshd_config
```
Edit the file as follows:
```sh
Permit root login: no
Password Authentication: no
```
Restart ```ssh``` to apply these changes by typing:
```sh
sudo service ssh restart
```
### Step 6: Configure the Firewall on your machine

FINDMINE machines use [UFW (Uncomplicated FireWall)][UFW] as the firewall configuration tool. You will need to enable this service by typing:

```shell
sudo ufw enable
```

Then, depending on the needs of your machine, configure the firewall to allow the specific ports. The command to allow a port is:

```shell
sudo ufw allow <port>/<optional: protocol>
```

You will most likely need to configure the necessary ports for redis: ```7000``` and ```7001``` in order for your FINDMINE instance to work.

### Step 7: Install all the necessary dependencies for FINDMINE

Start out by typing
```sh
sudo apt-get update && sudo apt-get upgrade -y
```
Check out the file ```setup.sh``` in the FINDMINE project for a list of necessary components, and install them using ```apt-get``` or ```pip3```.

####PhantomJS
PhantomJS, as it comes using ```apt-get```, requires XServer as a dependency. Since our machine is a virtual linux machine that only has terminal, we need to get around this built in dependency.

To get PhantomJS correctly installed and configured on your new machine, start by installing PhantomJS using the command,
```sh
sudo apt-get install phantomjs -y
```
Then go to the PhantomJS [webpage][phantom] and copy the download link for Linux onto your clipboard.

On your machine, type
```shell
wget url.for.downloading/phantom
```
Once the tar has been downloaded, untar it with the command
```shell
tar xvf phantom.tar
```
Enter the directory of the untarred phantom.

You want to move the executable into the directory where phantomjs is kept. While still in the untarred phantom directory,  find that location (in this tutorial, it is ```/usr/bin```), and move your executable into that folder.
```sh
$ which phantomjs
> /usr/bin
$ cd bin
$ sudo mv phantomjs /usr/bin/
```
After moving the executable, to tidy up, delete the untarred folder.

### Extra Notes:

If you have a custom vimrc, tmux bash_aliases and gitignore that you like, make sure to those files and folders over to the new machine!

### Next Steps:
Make sure that this new machine has an OpsDash agent running on it so that it can be monitored. To do this, checkout FINDMINE's OpsDash documentation.

##Conclusion
You now have a new machine! You can add more users with the following commands:
```sh
$ sudo adduser USERNAME
$ cp -r .ssh /home/USERNAME
$ chown -R USERNAME:USERNAME .ssh/
$ cd .ssh/
$ chmod 400 authorized_keys
```
If you want to make this new user a superuser, type:
```sh
usermod -a -G sudo USERNAME
```
Verify that this command was successful by typing:
```sh
groups USERNAME
> USERNAME sudo
```

If you have any further questions (or comments about this documentation), email Angela at angela.fox@findmine.com


[softlayer]: <https://control.softlayer.com/>
[phantom]:<http://phantomjs.org/download.html>
[UFW]:<https://help.ubuntu.com/community/UFW>














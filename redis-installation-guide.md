# FINDMINE: Installing Redis on your Computer

Redis is an in-memory key-value store known for its flexibility, performance, and wide language support. This guide should help you to install redis on your personal computer so that you can run the FINDMINE app locally.

Note: This tutorial has been tested on Linux Mint, and was built based on the Ubuntu 16.04 machine, and is based off [this tutorial][tutorial]

### Prerequisites
To complete this guide, you will need sudo privileges and access to FINDMINE's webservers.

### Install the Build and Test Dependencies
Update the apt package cache and install dependencies by typing:

```sh
$ sudo apt-get update
$ sudo apt-get install build-essential tcl
```
#### Download, Compile and Install Redis
Next we can begin to build Redis.

##### Download and Extract the Source Code
We will build redis in the /tmp directory and download and untar the latest stable version of Redis. This is always available at [a stable download URL][stable]:

```sh
$ cd /tmp
$ curl -O http://download.redis.io/redis-stable.tar.gz
$ tar xzvf redis-stable.tar.gz
```
Move into the Redis source directory structure that was just extraced:
```sh
$ cd redis-stable
```

##### Build and Install Redis
Now we can compile the Redis binaries by typing
```sh
$ make
```
Then, check that everything was built correctly by typing:

```sh
$ make test
```
This may take a few minutes to run. Once it's done, you can install the binaries onto the system by typing:

```sh
$ sudo make install
```
#### Configure Redis
To start off, create a configuration directory. We will use the conventional /etc/redis directory. 

```sh
$ sudo mkdir /etc/redis
```
Now, copy over the [Redis configuration file][googledoc] that is in the FINDMINE Engineering Folder on Google Drive

```sh
$ sudo cp /path/to/redis.conf /etc/redis
```

#### Create a Redis systemd Unit File
We need to create a systemd unit file so that the init system can manage the Redis process. Create this file by copying over the [Redis systemd unit file][redis.service] that is in the FINDMINE Engineering Folder on Google Drive.

```sh
$ sudo cp /path/to/redis.service /etc/systemd/system/redis.service
```

#### Create the Redis User, Group and Directories
Now we just have to create the user, group and directories that we reference in the previous files.

Begin by creating the redis user and group by typing:
```sh
$ sudo add user --system --group --no-create-home redis
```
Note: Even if you create multiple instances of redis on one machine, you will still only need one redis user who will own all the relevant directories.

Now we can create the ```/var/lib/redis``` directory and give `redis` the user and group ownership over this directory:
```sh
$ sudo mkdir /var/lib/redis
$ sudo chown redis:redis /var/lib/redis
```
Next we create the ```/var/run/redis``` directory and give `redis` the user and group ownership over this directory:
```sh
$ sudo mkdir /var/run/redis
$ sudo chown redis:redis /var/run/redis
```

Last, we create the `/var/log/redis` directory and give `redis` the user and group ownership over it:
```sh
$ sudo mkdir /var/log/redis
$ sudo chown redis:redis /var/log/redis
```

#### Start and Test Redis
Now we are ready to start the Redis server.

Start up the systemd service by typing:
```sh
$ sudo systemctl start redis
```
Check that the service had no errors by running:
```sh
$ sudo systemctl status redis
```
You should see something like this:
 ```
   ● redis.service - Redis Datastore Server
   Loaded: loaded (/etc/systemd/system/redis.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2016-09-06 11:07:16 EDT; 48min ago
   Process: 18098 ExecStop=/usr/local/bin/redis-cli -a 128a83d7 shutdown (code=exited, status=0/SUCCESS)
   Main PID: 18235 (redis-server)
   CGroup: /system.slice/redis.service
           └─18235 /usr/local/bin/redis-server 127.0.0.1:6379       
```

#### Test the Redis Instance Functionality
To test that your service is functioning correctly, connect to the Redis server wiht the command-line client

```sh
$ redis-cli
```
You will need to authorize yourself every time you try to make redis commands on your local computer. On the servers, this is unnessecary. Do this by typing `auth` followed by the necessary password.

```sh
127.0.0.1:6379> auth *********
```

In the prompt that follows, test connectivity by typing:
```sh
127.0.0.1:6379> ping
```
You should see:
```
Output: PONG
```
Check that you can set keys by typing:
```sh
127.0.0.1:6379> set test "It's working!"
```
```
Output OK
```
Now retrieve the value by typing:
```sh
127.0.0.1:6379> get test
```
You should see: 
```sh
Output "It's Working"
```
Now exit the Redis prompt, restart Redis, reconnect with the client again and confirm that your test value is still available:

```sh
127.0.0.1:6379> exit
$ sudo systemctl restart redis
$ redis-cli
127.0.0.1:6379> get test
```

```
Output: "It's working"
```

#### Enable Redis to Start at Boot
If all your tests worked, and you'd like to start Redis automatically when you boot, you can enable the systemd service:

```sh
$ sudo systemctl enable redis
```

### Conclusion
You should now have a Redis instance installed and configured! If you have any issues with this tutorial, email Angela at angela.fox@findmine.com

[tutorial]: <https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04>
[stable]: <http://download.redis.io/redis-stable.tar.gz>
[googledoc]: <https://drive.google.com/a/findmine.com/file/d/0B5c9zj-N1jWnUjJJbm0tbk85OXc/view?usp=sharing>
[redis.service]: <https://drive.google.com/a/findmine.com/file/d/0B5c9zj-N1jWnU1VPYnplUVZiOHc/view?usp=sharing>

 

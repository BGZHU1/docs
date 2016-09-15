# FINDMINE Scrift Process: Store 11

Scrift stands for the combination of scraping and sifting feeds that our clients provide us with their site updates. 

All the necessary files can be accessed at Mishki's scrift repository on [Github][git].

## Step 1: Fetch
The first step in this process is to run ```fetch.sh```.
 This script copies all the files from the sftp directory to your own personal feed folder.
##### Before Running:
Depending on your file system configuration, you may need to adjust the path specified in fetch. You can alternatively add your own personal configuration to the script.
##### Run fetch:
Now, run the fetch script.
```sh
$ ./fetch.sh
```
##### After Running:
* Validate that the files were fetched and copied correctly by comparing the timestamp and file size with the originals in the sftp directory
* Commit the files from sftp to github.
```sh
username@sftp1:/home/store/feed$ git commit feed_file_timestamp.csv origin master
```
* Delete the files from sftp.
```sh
username@sftp1:/home/store/feed$ rm *.csv
```
## Step 2: To JSON
You do NOT need to run ```to_json.py``` as it gets called in ```to_dsl.py``` but it is useful if you would like to learn more about the feeds. 

Be aware that if you run ```to_json.py``` multiple times with different arguments specified, the file ```latest.json``` will be overwritten each time. 

****
```to_json.py``` for Store 11 can be run with the following args:
##### Path:
The path argument option allows you to specify a specific path to the feed file if you do not want to use the default configuration. **The default configuration, which happens if no path argument is supplied, is to select the *latest* feed file**.
```sh
$ ./to_json.py --path /some/specific/path
```
```sh
$ ./to_json.py -p /some/specific/path
```
##### Print:
The print argument, if specified, prints the JSON objects to the screen instead of writing them to a file.
```
$ ./to_json.py --print
```
##### Old:
If you run ```to_json.py``` with the ```old``` argument specified, ```to_json.py``` will return a file that contains all the items which were in the feed file *before* the **latest** feed file (assuming that the default path is specified) but that are *not* in the **latest** feed file. These items will have been converted from the feed format into a JSON format and stored in the file ```latest.json```.

To run ```to_json.py``` with the ```old``` argument specified, simply type:
```sh
$ ./to_json.py --old
```
or
```sh
$ ./to_json.py --o
```
##### New:
If you run ```to_json.py``` with the ```new``` argument specified, ```to_json.py``` will return a file that contains all the items which are in the **latest** feed file (assuming that the default path is specified) but that were not in the feed file *before* the **latest** feed file. These items will have been converted from the feed format into a JSON format and stored in the file ```latest.json```.

To run ```to_json.py``` with the ```new``` argument specified, simply type:
```sh
$ ./to_json.py --new
```
or
```sh
$ ./to_json.py --n
```
##### Live:
If you run ```to_json.py``` with the ```live``` argument specified, ```to_json.py``` will return a file that contains all the items which are currently **live**. These items will have been converted from the feed format into a a JSON format and stored in the file ```latest.json```.

To run ```to_json.py``` with the ```new``` argument specified, simply type:
```sh
$ ./to_json.py --live
```
or
```sh
$ ./to_json.py --l
```
## Step 3: To DSL
```to_dsl.py``` takes the feed files and converts them to  .dsl file (by first calling ```./to_json.py``` without the any args specified), in order to put them in the right format for pushing to our elastic search.

**Note: Running to_dsl.py will overwrite the file ```latest.json``` that may have been created by running ```./to_json.py``` with arguments ```new```, ```old```, or ```live```. **

To run to_dsl.py, simply type:
```sh
$ ./to_dsl.py
```

When ```to_dsl.py``` has completed execution, you will have a new file called ```latest.dsl``` in your working directory.
****
As with to_json.py, there are various additional arguments that you can specify if desired. These are described below:

#### Path:
The path argument option allows you to specify a specific path to the feed directory if you do not want to use the default configuration. **The default configuration, which happens if no path argument is supplied, is to select the ```feed``` directory**.
```
$ ./to_json.py --path /some/specific/path
```

```
$ ./to_json.py -p /some/specific/path
```
#### Print:
The print argument, if specified, prints the JSON objects to the screen instead of writing them to a file.
```
$ ./to_json.py --print 
```
## Step 4: Push Feed to Elastic Search Script
Next, you need to run the ```push_elastic.sh``` script which, when run without arguments, takes the ```latest.dsl``` file created from the feed and pushes it to elastic search on the **dev server**. 

In most cases, you will want to push to the elastic search of both the dev and the production servers. This means running the ```push_elastic.sh``` script like so:

```sh
$ ./push_elastic.sh
```
which will push to the dev server, followed by

```sh
$ ./push_elastic.sh -h 127.0.0.1
```
replacing ```localhost 127.0.0.1``` with the production server IP address.

****
```to_elastic.sh``` has the following optional arguments:

#### Filename
If you want to specify a filename that is different than the default ```latest.dsl``` file, you can do so by running
```sh
$ ./push_elastic.sh --file filename.dsl
```
or 
```sh
$ ./push_elastic.sh -f filename.dsl
```

#### Host
The default configuration pushes the ```latest.dsl``` file to the dev server. However, if you want to push to the production server, you should specify the production server host address.

```sh
$ ./push_elastic.sh --host 127.0.0.1
```
or 
```sh
$ ./push_elastic.sh -h 127.0.0.1
```

####Port
The default configuration pushes the ```latest.dsl``` file to the dev server, which is listening for elastic search on ```PORT 9200```. However, if you want to a different port number, you can do so using the port argument.

```sh
$ ./push_elastic.sh --port 8888
```
or 
```sh
$ ./push_elastic.sh -p 8888
```

## Step 5: To SQL
```to_sql.py``` calls ```to_json.py``` internally and determines which items are in or out of stock and updates the stock value stored in the mySQL database. It writes a file called ```latest.sql``` to the working directory.

Run ```to_sql.py``` by typing:
```sh
$ ./to_sql.py
```
***

```to_sql.py``` for Store 11 can be run with the following args:
##### Path:
The path argument option allows you to specify a specific path to the feed file if you do not want to use the default configuration. **The default configuration, which happens if no path argument is supplied, is to select the *latest* feed file**.
```sh
$ ./to_sql.py --path /some/specific/path
```
```sh
$ ./to_sql.py -p /some/specific/path
```
##### Print:
The print argument, if specified, prints the JSON objects to the screen instead of writing them to a file.
```
$ ./to_sql.py --print
```

## Push mySQL

Next, you need to run the ```push_mysql.sh``` script which, when run without arguments, takes the ```latest.sql``` file created from the feed and pushes it to mysql database on the **dev server**. 

In most cases, you will want to push to the mysql database of both the dev and the production servers. This means running the ```push_mysql.sh``` script like so:

```sh
$ ./push_mysql.sh -u **********
```
Here, specify the relevant username. **When prompted, enter the correct password.**

Now that you have pushed to the dev server, you need to push to the production server.

```sh
$ ./push_mysql.sh -h 127.0.0.1 -u *********
```
replacing ```localhost 127.0.0.1``` with the production server IP address, and adding the relevant username. **When prompted, enter the correct password.**

****
```to_mysql.sh``` has the following optional arguments:

#### Filename
If you want to specify a filename that is different than the default ```latest.sql``` file, you can do so by running
```sh
$ ./push_mysql.sh --file filename.sql
```
or 
```sh
$ ./push_mysql.sh -f filename.sql
```

#### Host
The default configuration pushes the ```latest.sql``` file to the dev server. However, if you want to push to the production server, you should specify the production server host address.

```sh
$ ./push_mysql.sh --host 127.0.0.1
```
or 
```sh
$ ./push_mysql.sh -h 127.0.0.1
```

####Username
The default configuration uses the default username trivigy. However, if you want to use your own username and password, you can specify a different username using the ```username``` argument.

```sh
$ ./push_mysql.sh --username ******
```
or 
```sh
$ ./push_elastic.sh -u *******
```

## Notify FINDMINE Administrator
FINDMINE Administrators greatly appreciate it if you send them a json file with the new items from the latest feed. This way they know which items need outfits made and reviewed. 

To do this, run the following command:
```sh
$ ./to_json.py --new
```
Remember that this will overwrite ```latest.json```.

Email a FINDMINE Administrator with the ```latest.json``` file attached.

## Step 5: Update the cache in Redis and Elastic Search

**NOTE: If you are completing the scrift process for multiple stores, you only need to run Step 5 once after completing Steps 1 - 4 for each store.**

### Step 5.1 Fetch Redis
The ```fetch_redis.py``` program creates a ```latest.redis``` file in the current working directory. 

First, enter the ```cache``` directory:
```sh
$ cd ../cache
```
Then run the ```fetch_redis.py``` program:
```sh
$ ./fetch_redis.py
```
### Step 5.2 Push Redis
The ```push_redis.sh``` script pushes ```latest.redis``` to the dev server cache by default. In most situations, you will want to run ```push_redis.sh``` pointing both at dev and at the production server.

Do this by running:
```sh
$ ./push_redis.sh
```
```sh
$ ./push_redis.sh -h 127.0.0.1
```
Change ```127.0.0.1``` to the correct production server IP address.
*****

As with ```push_elastic.sh```, ```push_redis.sh``` has the following optional arguments:

#### Filename
If you want to specify a filename that is different than the default ```latest.redis``` file, you can do so by running
```sh
$ ./push_redis.sh --file filename.redis
```
or 
```sh
$ ./push_redis.sh -f filename.redis
```

#### Host
The default configuration pushes the ```latest.redis``` file to the dev server. However, if you want to push to the production server, you should specify the production server host address.

```sh
$ ./push_redis.sh --host 127.0.0.1
```
or 
```sh
$ ./push_redis.sh -h 127.0.0.1
```

####Port
The default configuration pushes the ```latest.redis``` file to the dev server, which is listening for redis on ```PORT 7001```. However, if you want to a different port number, you can do so using the port argument.

```sh
$ ./push_redis.sh --port 8888
```
or 
```sh
$ ./push_redis.sh -p 8888
```


### Step 5.3 Fetch Elastic
The ```fetch_elastic.py``` in the ```cache``` directory selects  the ```item_id```, ```title```, ```item_url```, ```product_id``` and ```description``` for every item in the mysql database on the **production** server and creates a file ```latest.dsl``` with JSON objects for all the items.

To run, simply type the following command in the ```cache``` directory:
```sh
$ ./fetch_elastic.py
```

### Step 5.4 Push Elastic

The ```push_elastic.sh``` script pushes ```latest.dsl``` made up of all items from the **production mysql database**to the dev server elastic search by default. In most situations, you will want to run ```push_elastic.sh``` pointing both at dev and at the production server.

Do this by running:
```sh
$ ./push_elastic.sh
```
```sh
$ ./push_elastic.sh -h 127.0.0.1
```
Change ```127.0.0.1``` to the correct production server IP address.
*****

 ```push_elastic.sh``` has the following optional arguments:

#### Filename
If you want to specify a filename that is different than the default ```latest.dsl``` file, you can do so by running
```sh
$ ./push_elastic.sh --file filename.dsl
```
or 
```sh
$ ./push_elastic.sh -f filename.dsl
```

#### Host
The default configuration pushes the ```latest.dsl``` file to the dev server. However, if you want to push to the production server, you should specify the production server host address.

```sh
$ ./push_elastic.sh --host 127.0.0.1
```
or 
```sh
$ ./push_elastic.sh -h 127.0.0.1
```

####Port
The default configuration pushes the ```latest.dsl``` file to the dev server, which is listening for elastic search on ```PORT 9200```. However, if you want to a different port number, you can do so using the port argument.

```sh
$ ./push_elastic.sh --port 8888
```
or 
```sh
$ ./push_elastic.sh -p 8888
```

## Conclusion
You have now successfully performed the scrift process for Store 11. If you have any issues with this documentation, email Angela at angela.fox@findmine.com.







[git]: <https://github.com/Mishki/scrift>













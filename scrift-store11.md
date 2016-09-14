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
1. Validate that the files were fetched and copied correctly by comparing the timestamp and file size with the originals in the sftp directory
2. Commit the files from sftp to github.
```sh
username@sftp1:/home/store/feed$ git commit feed_file_timestamp.csv origin master
```
3. Delete the files from sftp.
```sh
username@sftp1:/home/store/feed$ rm *.csv
```
## Step 2: To JSON
You do NOT need to run ```to_json.py``` as it gets called in ```to_dsl.py``` but it is useful if you would like to learn more about the feeds.

### to_json.py for STORE 11:
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






[git]: <https://github.com/Mishki/scrift>













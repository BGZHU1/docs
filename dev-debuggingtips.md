# Dev: Debugging Tips

## Running FINDMINE on your personal computer

### Tunneling
#### Tunneling Issue: # 1:  ```Remote Port Forwarding Failed For Listen Port ####```

If you are trying to tunnel to run FINDMINE on your personal computer, you may see the following error after typing:
```sh
$ ./dev1.wdc.sh <PortNumber>
Remote Port Forwarding Failed for Listen Port <PortNumber>
```
Here are some good troubleshooting tips
First, log into dev
```sh
$ ./dev1.wdc.sh
```
Then, run the following command to see if there are extra, unnessecary ssh sessions
```sh
$ who -a
```
If you do see unnessecary ssh connections, you can kill them using the command:
```sh
$ sudo kill -9 <ssh ports>
```
This error could also occur because you have lingering connection on the desired port. Use ```netstat``` to see if the dev server is already listening  on the port
```sh
$ netstat -l
```
If dev is listening on that port, clear it using the following command:
```sh
$ sudo fuser -k <portnumber>/tcp
```
Now try to tunnel again!





If you have any issues with this documentation, email Angela at angela.fox@findmine.com

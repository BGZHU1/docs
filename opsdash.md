# OpsDash Documentation for FINDMINE

### Adding a new agent to an existing OpsDash Server

#### Introduction

FINDMINE uses OpsDash to monitor their servers and their servers associated services, with dashboarding and alerting functionality. Whenever you create a new machine, you should also install OpsDash agent and configure it to the appropriate OpsDash server.

#### Install OpsDash Agent

Start by installing the OpsDash agent onto the machine that you want to monitor. The following documentation is an abridged version of the OpsDash documentation, which can be viewed [here][opsdash], under the heading 'Using the RapidLoop apt repo'.

First, create the file ```/etc/apt/source.list.d/rapidloop.list``` with the following contents:

```shell
deb http://packages.rapidloop.com/debian stable main
```

Import the RapidLoop signing key:

```sh
apt-key adv --keyserver pgp.mit.edu --recv-keys 1E1BACF5
```

Update the local metadata:

```shell
apt-get update
```

Install the OpsDash Agent:

```sh
# install the package
apt-get install opsdash-agent
```

View the configuration file, and set the server field equal to the machine which is running the OpsDash server. This is either the dev OpsDash server running on dev1, or the production OpsDash running on the load balancer machine.

```sh
# review configuration file
vim /etc/opsdash/agent.cfg
```

```
server = IP.of.OPSDASH.server
```

You can leave all the other settings as they are.

#### Start the OpsDash Agent

```shell
# start the daemon
service opsdash-agentd start
```

#### Verify that your agent is live

It can take about two minutes for your newly created OpsDash Agent to be live on the dashboard. 

Go to either the [production dashboard][production], or the [dev dashboard][dev] and checkout your new OpsDash agent.

## Conclusion

If you have any questions or comments regarding this documentation, contact Angela at angela.fox@findmine.com





[opsdash]: https://www.opsdash.com/docs/install.html#quick-start	"opsdash documentation"

[production]: www.findmine.com:6443	"production"

[dev]: dev.findmine.com:6443	"dev"
#Nettest agent

This is a simple agent that will execute a ping or remote connection test on mcollective hosts

I often find myself logging onto boxes to ping different sites to diagnose local or remote network issues, this means I can now just issue a single command and get results from anywhere I’m running mcollective.

##Installation

* Install RubyGem [Net::Ping](http://raa.ruby-lang.org/project/net-ping/)
* Follow the [basic plugin install guide](http://projects.puppetlabs.com/projects/mcollective-plugins/wiki/InstalingPlugins)

##Usage

```
$ mco rpc nettest ping fqdn="hostname"
```

```
$ mco rpc nettest connect fqdn="hostname" port="nn"
```

```
[root@host1 agent]$ mco rpc nettest ping fqdn=google.com
  Discovering hosts using the mc method for 2 second(s) .... 1

    * [ ============================================================> ] 1 / 1


    host1.example.com
      RTT: 0.319


    Summary of RTT:

       Min: 0.319ms  Max: 0.319ms  Average: 0.319ms
```

###Validator

The nettest agent supplies an fqdn validator which will validate if a string is a valid uri.

```
validate :fqdn, :fqdn
```
###Data Plugin

The nettest agent also supplies a data plugin which uses the nettest agent to check if a connection to a fqdn at a specific port can be made. The data plugin will return 'true' or 'false' and can be used during discovery or any other place where the MCollective discovery language is used.

```
mco rpc rpcutil -S "Nettest('myhost', '8080').connect=true"
```
###Mma Aggregate Plugin

The nettest agent also ships with a mma aggregate plugin which will determine the minimum value, maximum value and average value of a set of inputs determinted in a DDL.
```
aggregate mma(:rtt, :format => "Min: %.3fms  Max: %.3fms  Average: %.3fms")
```

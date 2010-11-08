This is a basic dynamic client and server for the djbdns DNS 
server 'tinydns' by Dan Bernstein.

## Requirements:

- Administrative access to a server running tinydns and hence
  daemontools or similar
- Administrative access to a \*nix box on you local network
- Python installed on both client and server
- Access to firewall (default port: 5300 TCP)

## Installation

To install the server code:

1. Copy script and all files onto your server (i.e. /etc/tinydnsdyn/)
2. The supplied 'run' script is tailored for debian/ubuntu and may
   need to be changed.
3. Create a password file that contains the password on the first line
4. Start the service by symlinking this directory into your svscan
   directory (i.e. "ln $(pwd) /etc/service/" ).
5. An existing entry in the tinydns data file should already exist.
   This script will NOT add one. Only those hosts listed 
   (with prefixes '+' and '=') are able to update their entries.

For example:

    +groucho.example.com:192.168.0.2:3360
    +groucho.example.com:192.168.0.2
    =groucho.example.com:192.168.0.2:60
    +*.groucho.example.com:192.168.0.2:60
    +*.groucho.example.com:192.168.0.2

will all get updated if the host groucho.example.com does a request.
   

To install the client code:

1. Copy the 'tinydnsdyn-client' script into your DNS client's path
   (i.e. /usr/local/bin/)
2. Arrange to periodically run the 'tinydnsdyn-client' program (via
   cron et al) like so:

         tinydnsdyn-client --hostname=<myhostname> --server=<mydnsserver> <passfile>

Only the passfile option is required.

3. The password must obviously be the same and the requested host name
   must exist in the 'data' file.

The Python code is basic and inefficient but it should work. I take no
responsibility for anything that may happen to any of your machines.
That said, please let me know if there are any issues by creating a ticket on [my tracker](http://support.seconddrawer.com/projects/tinydnsdyn/).



###FreeBSD
On FreeBSD 8.1, the following install steps seemed to work. Using the FreeBSD branch here:

https://github.com/tehfink/tinydnsdyn

To install on server:

    cd /usr/local/etc/
    git clone git://github.com/tehfink/tinydnsdyn.git
    cd tinydnsdyn
    git checkout freebsd
    echo "yourpasswordhere" > /usr/local/etc/tinydnsdyn/passfile
    ln -s /usr/local/etc/tinydnsdyn/tinydnsdyn /usr/local/bin/tinydnsdyn
    ln -s /usr/local/etc/tinydnsdyn /var/service/
    sleep 5

To install on client:

    cd /usr/local/etc/
    git clone git://github.com/tehfink/tinydnsdyn.git
    cd tinydnsdyn
    git checkout freebsd
    echo "yourpasswordhere" > /usr/local/etc/tinydnsdyn/passfile
    ln -s /usr/local/etc/tinydnsdyn/tinydnsdyn-client /usr/local/bin/tinydnsdyn-client
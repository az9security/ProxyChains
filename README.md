# ProxyChains
ProxyChains
Installing Tor & Proxychains in Linux

First, update the Linux system with the patches and the latest applications. For this we open a terminal and type:

$ sudo apt update && sudo apt upgrade

Then check whether Tor and Proxychains are pre-installed or not by simply typing these commands separately :

$ proxychains 

$ tor

If they were not installed, type the following command in the terminal:

$ sudo apt install proxychains tor -y

Please note that we’re not installing the Tor browser. We’re installing the tor service which is a service that runs locally on your virtual machine or on your operating system and is actually bound to a particular port on local-host. In our case, it’s going to be 9050 and that’s the default with the tor service.

To check the status of Tor :

┌──(root💀kali)-[/home/writer]
└─# service tor status                                                                 
● tor.service - Anonymizing overlay network for TCP (multi-instance-master)
     Loaded: loaded (/lib/systemd/system/tor.service; disabled; vendor preset: disabled)
     Active: inactive (dead)

To start the tor service :

$ service tor start

To stop the tor service :

$ service tor stop

Configuring ProxyChains

First, locate the directory of ProxyChains by using this command :

┌──(root💀kali)-[~]
└─# locate proxychains                       
/etc/proxychains4.conf
/etc/alternatives/proxychains
/etc/alternatives/proxychains.1.gz
/usr/bin/proxychains
/usr/bin/proxychains4
/usr/lib/x86_64-linux-gnu/libproxychains.so.4
/usr/share/applications/kali-proxychains.desktop
/usr/share/doc/libproxychains4
/usr/share/doc/proxychains4

This is our configuration file.

/etc/proxychains4.conf

Based on the above result, we can notice that the ProxyChain config file is located in /etc/.

We need to make some adjustments to ProxyChains configuration files. Open the config file in your favorite text editor like leafpad,  vim, or nano.

Here I am using nano editor.

nano /etc/proxychains.conf

proxy2

The config file is opened. Now you need to comment and comment out some lines to set up the proxy chains.

You’ll notice “#” in the configuration, which stands for bash language comments. You may scroll down and make the adjustments using the arrow keys.

#1. Dynamic chain should be removed from the remark comment. All you have to do is to remove a # in front of dynamic_chain.

dynamic_chain
#
# Dynamic - Each connection will be done via chained proxies
# all proxies chained in the order as they appear in the list
# at least one proxy must be online to play in chain
# (dead proxies are skipped)
# otherwise EINTR is returned to the app

#2. Put the comment in front of random_chain and strict_chain. Just add # in front of these.

#random_chain
#
# Random - Each connection will be done via random proxy
# (or proxy chain, see  chain_len) from the list.
# this option is good to test your IDS :)

#3. Max times it includes the proxy-DNS uncomment, double-check that it is uncommented. You will avoid any DNS leaks that may reveal your true IP address in this manner.

# Proxy DNS requests - no leak for DNS data
proxy_dns
 

#4. Add socks5 127.0.0.1 9050 in the proxy list the last line.

[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks4  127.0.0.1 9050 
socks5  127.0.0.1 9050 

Here socks4 proxy will be already given. You need to add the socks5 proxy as shown above. And finally, save the config file and exit the terminal.
Usage of ProxyChains

At first, you have to start the Tor service in order to use ProxyChains.

┌──(root💀kali)-[/home/writer]
└─# service tor start

After the tor service is started, you can use ProxyChains for browsing and for anonymous scanning and enumeration. You can also use Nmap or sqlmap tool with ProxyChain for scanning and searching exploits anonymously. It’s great, right?

To utilize ProxyChains, simply type the ProxyChains command in a terminal, followed by the name of the app you want to use. The format is as follows:

┌──(writer㉿kali)-[~]
└─$ proxychains firefox www.flippa.com

To use Nmap:

$ proxychains nmap -targetaddress

To use sqlmap:

$  proxychains python sqlmap -u target

You can also test for exploits anonymously like

$ proxychains python sqlmap -u http://www.targetaddress/products.php?product=3

Literally, Every TCP reconnaissance tool can be used with ProxyChains.

For the final confirmation of ProxyChains is working properly or not, just go to dnsleaktest.com and check your IP address and DNS leaks.

After running ProxyChains, you will notice that Firefox has been loaded with a different language. Now, let’s perform a DNS leak test by using a command :

$ proxychains firefox dnsleaktest.com

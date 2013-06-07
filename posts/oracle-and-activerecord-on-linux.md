---
title: Oracle and ActiveRecord on Linux
date: '2012-04-04'
description:
tags: []
---

So, I've been playing with [Vagrant][1] over the last day or so in the hope that we can use it to speed up the time it takes to get new starters up and running and also to speed up our cucumber test suite for the windows users among our team.

I am hoping that we will be able to spin up a light weight Linux distro with Ruby installed, allowing team members to run their features from there. This means we will no longer need to muck around installing Ruby on Windows (which is a pain) and hopefully it runs faster than it does on Windows.

In setting up this Vagrant box I came across the same issue that I did when setting cucumber up for our project on my Mac. Oracle causes issues with ActiveRecord. So, I thought I'd record, here, the way I fixed this once and for all so I don't need to go searching the net for it again.

So, starting out with a vanilla install of Ubuntu Lucid Lynx with only `rbenv` and `ruby` installed, I did the following:

- Download `instantclient-basic-linux-11.2.0.3.0.zip` and `instantclient-sdk-linux-11.2.0.3.0.zip` and copy them to the Vagrant project directory.

- Run the following commands:

```bash
digital-alley:tmpdir aaron$ vagrant up
digital-alley:tmpdir aaron$ vagrant ssh
 
vagrant@lucid32:~$ sudo apt-get install unzip
vagrant@lucid32:~$ sudo apt-get install rpm
vagrant@lucid32:~$ cd /tmp
vagrant@lucid32:~$ cp /vagrant/instantclient-* .
vagrant@lucid32:~$ cd /opt
vagrant@lucid32:~$ sudo mkdir oracle
vagrant@lucid32:~$ cd oracle
vagrant@lucid32:~$ sudo unzip /tmp/instantclient-basic-linux-11.2.0.3.0.zip
vagrant@lucid32:~$ sudo unzip /tmp/instantclient-sdk-linux-11.2.0.3.0.zip
vagrant@lucid32:~$ cd instantclient_11_2
vagrant@lucid32:~$ sudo ln -s libclntsh.so.11.1 libclntsh.so
vagrant@lucid32:~$ LD_LIBRARY_PATH=/opt/oracle/instantclient_11_2
vagrant@lucid32:~$ export LD_LIBRARY_PATH
vagrant@lucid32:~$ cd /vagrant
vagrant@lucid32:~$ gem install ruby-oci8
vagrant@lucid32:~$ sudo apt-get install libaio-dev
```

- To test that this all worked, run the following:

```bash
vagrant@lucid32:~$ irb
 
irb(main):001:0> require 'rubygems'
irb(main):001:0> require 'oci8'
```

And there you have it!

PS, A couple of props to the sites that helped me achieve this solution:

[http://www.darianshimy.com/2011/09/ruby-oci8-library-problems][2]  
[http://ruby-oci8.rubyforge.org/en/InstallForInstantClient.html][3]

[1]: http://vagrantup.com/ "Vagrant"
[2]: http://www.darianshimy.com/2011/09/ruby-oci8-library-problems/
[3]: http://ruby-oci8.rubyforge.org/en/InstallForInstantClient.html
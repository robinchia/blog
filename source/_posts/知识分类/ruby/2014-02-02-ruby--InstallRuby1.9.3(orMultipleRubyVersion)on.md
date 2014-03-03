---
layout: post
title: "Install Ruby 1.9.3 ( or Multiple Ruby Version ) on"
categories: ruby
tags: 
 - ruby
--- 

# Install Ruby 1.9.3 ( or Multiple Ruby Version ) on CentOS, RedHat using RVM

I am using CentOS 6.3, below is easy steps to install ruby using RVM , or Ruby Version Manager.  RVM provides easy set of commands to install single or multiple versions of Ruby on single server.

### Step 1: Upgrade Packages

It is the best practice to keep your system up to date with latest packages. Before running below command make sure that update will not affect to your running apps ( if any )on server else skip it
# yumupdate

### Step 2: Installing Recommended Packages

There are few developemnt libraries required to run Ruby on Linux. Use following command to install recommended packages on your server using yum.
# yuminstallgcc-c++ patch readline readline-devel zlib zlib-devel  # yuminstalllibyaml-devel libffi-devel openssl-devel make  # yuminstallbzip2 autoconf automake libtool bison iconv-devel

### Step 3: Install RVM ( Ruby Version Manager )

Install latest stable version of RVM on your system using following command. This command will automatically download all required files and install on your system.
# curl -Lget.rvm.io| bash -sstable

[Sample Output]

[root@redhat6 beef]# rvm install 1.9.3                                                                                 

bash: rvm: command not found                                                                                           

[root@redhat6 beef]# curl -L get.rvm.io | bash -s stable

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current

                                 Dload  Upload   Total   Spent    Left  Speed  

100 20511  100 20511    0     0  11534      0  0:00:01  0:00:01 --:--:--  175k 

Downloading https://github.com/wayneeseguin/rvm/archive/stable.tar.gz          

Creating group 'rvm'                                                           

Installing RVM to /usr/local/rvm/                                                                                        

Installation of RVM in /usr/local/rvm/ is almost complete:                                                               

  * First you need to add all users that will be using rvm to 'rvm' group,

    and logout - login again, anyone using rvm will be operating with `umask u=rwx,g=rwx,o=rx`.

  * To start using RVM you need to run `source /etc/profile.d/rvm.sh`

    in all your open shell windows, in rare cases you need to reopen all shell windows.

# robin,

#       

#   Thank you for using RVM!

#   We sincerely hope that RVM helps to make your life easier and more enjoyable!!!

#                                                                                  

# ~Wayne, Michal & team.                                                           

In case of problems: http://rvm.io/help and https://twitter.com/rvm_io

### Step 4: Setup RVM Environment

After installing RVM first we need to setup rvm environment using below command.
# source/etc/profile.d/rvm.sh

### Step 5: Install Required Ruby Version

RVM provides option to manage multiple ruby version on single system. Use following command to install required version of Ruby.
# rvm install1.9.3

[Sample Output]

[root@redhat6 soft]# rvm install 1.9.3                                                                  

Searching for binary rubies, this might take some time.                                                 

No binary rubies available for: redhat/6/x86_64/ruby-1.9.3-p484.                                        

Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.                      

Checking requirements for redhat.                                                                                        

Installing requirements for redhat.                                                                                      

Updating system.                                                                                                         

Installing required packages: libyaml-devel, libffi-devel........                                                        

Requirements installation successful.                                                                                    

Installing Ruby from source to: /usr/local/rvm/rubies/ruby-1.9.3-p484, this may take a while depending on your cpu(s)... 

ruby-1.9.3-p484 - #downloading ruby-1.9.3-p484, this may take a while depending on your connection...                    

Warning: Transient problem: timeout Will retry in 2 seconds. 3 retries left.

Warning: Transient problem: timeout Will retry in 2 seconds. 2 retries left.

Warning: Transient problem: timeout Will retry in 2 seconds. 1 retries left.

curl: (28) connect() timed out!

There was an error(28).        

Checking fallback: http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p484.tar.bz2

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current 

                                 Dload  Upload   Total   Spent    Left  Speed   

100 9806k  100 9806k    0     0   207k      0  0:00:47  0:00:47 --:--:--  239k  

ruby-1.9.3-p484 - #extracting ruby-1.9.3-p484 to /usr/local/rvm/src/ruby-1.9.3-p484.

ruby-1.9.3-p484 - #applying patch /usr/local/rvm/patches/ruby/GH-488.patch.         

ruby-1.9.3-p484 - #applying patch /usr/local/rvm/patches/ruby/ssl_no_ec2m.patch.    

ruby-1.9.3-p484 - #configuring.............................................         

ruby-1.9.3-p484 - #post-configuration.                                              

ruby-1.9.3-p484 - #compiling....................................................................

ruby-1.9.3-p484 - #installing........................                                           

ruby-1.9.3-p484 - #making binaries executable.                                                  

ruby-1.9.3-p484 - #downloading rubygems-2.2.2                                                   

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current                 

                                 Dload  Upload   Total   Spent    Left  Speed                   

100  404k  100  404k    0     0  39849      0  0:00:10  0:00:10 --:--:-- 41083                  

No checksum for downloaded archive, recording checksum in user configuration.                   

ruby-1.9.3-p484 - #extracting rubygems-2.2.2.                                                   

ruby-1.9.3-p484 - #removing old rubygems.                                                       

ruby-1.9.3-p484 - #installing rubygems-2.2.2...............                                     

ruby-1.9.3-p484 - #gemset created /usr/local/rvm/gems/ruby-1.9.3-p484@global                    

ruby-1.9.3-p484 - #importing gemset /usr/local/rvm/gemsets/global.gems.....                     

ruby-1.9.3-p484 - #generating global wrappers.                                                  

ruby-1.9.3-p484 - #gemset created /usr/local/rvm/gems/ruby-1.9.3-p484                           

ruby-1.9.3-p484 - #importing gemsetfile /usr/local/rvm/gemsets/default.gems evaluated to empty gem list

ruby-1.9.3-p484 - #generating default wrappers.                                                        

ruby-1.9.3-p484 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).                          

Install of ruby-1.9.3-p484 - #complete                                                                 

WARNING: Please be aware that you just installed a ruby that will finish normal maintenance on 2014-02-23, for a list of maintained rubies visit:                                                                                                 

                                                                                                                         

    http://bugs.ruby-lang.org/projects/ruby/wiki/ReleaseEngineering                                                      

                                                                                                                         

Please consider upgrading to ruby-2.1.0 which will have all of the latest security patches.                              

Ruby was built without documentation, to build it run: rvm docs generate-ri

### Step 6: Install Another Version ( if Required )

If you want using multiple versions of ruby, you can install it also using rvm else skip this step.
# rvm install1.8.6

### Step 7: Setup Default Ruby Version

Use rvm command to setup default ruby version to be used by applications.
#rvm use 1.9.3 --defaultUsing /usr/local/rvm/gems/ruby-1.9.3-p448

### Step 8: Check Current Ruby Version

Using following command you can check the current ruby version is used.
# ruby --versionruby 1.9.3p448 (2013-06-27 revision 41675) [i686-linux]

**Step 9:gem install eventmachine or  bundle install**

 Step 10: bundle show  

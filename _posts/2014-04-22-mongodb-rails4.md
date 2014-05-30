---
layout: post 
title: MongoDB and Mongoid for rails4 
keywords: mongodb, mongoid 
tags: mongoDB 
description: Today we are going to walk through installing it on Ubuntu and setting up Rails with MongoDB and Mongoid to replace ActiveRecord.
---
<h3>Getting Started</h3>
<p><b>Step 1:</b> install MongoDB from their official repository</p>

<pre>
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
$ sudo echo "deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen" | tee -a /etc/apt/sources.list.d/10gen.list
$ sudo apt-get -y update
$ sudo apt-get -y install mongodb-10gen
</pre>

<div class="notice">
  <p><b>Notice:</b></p>
  <p class="notice-text">
    if you got 'tee: /etc/apt/sources.list.d/10gen.list: 权限不够'
  </p>
  <p class="notice-text">
    you should got super user: '$ sudo su'
  </p>
  <p class="notice-text">
    then try above command again
  </p>
</div>

<p>Make sure you installed MongoDB: </p>

<pre> 
$ mongod
$ mongo
MongoDB shell version: 2.4.10
connecting to: test
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
&gt;
</pre>

<p><b>Step 2:</b> Generate your Rails application with 'rails new myapp --skip-active-record'</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;You will need to change these lines in your Gemfile: </p>

<pre>
# remove 
# Use sqlite3 as the database for Active Record
gem 'sqlite3'

# add
# mongodb
gem 'mongoid', git: 'git://github.com/mongoid/mongoid.git'
</pre>

<p><b>Step 3:</b> generate the Mongoid configuration file </p>

<pre>
$ rails g mongoid:config
</pre>

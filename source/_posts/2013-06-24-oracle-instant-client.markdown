---
layout: post
title: "oracle instant client"
date: 2013-06-24 21:00
comments: true
categories: 
---
<script type="text/javascript">

  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-38989132-1']);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();

</script>

***So today was my first day as a Software Engineer at Time Inc.*** I can already tell it's going to be such an incredible learning experience, and all we did was environment setup today.

__The hardest part about all of this, was setting up the client for an Oracle database.__ An Oracle database is an object-relational database management system, which means that it is similar to a relational-database but uses an object-oriented model. Essential, it supports objects, classes and inheritence directly in the database schemas and query language, as well as supports custom data-types and methods.

The process of installing took me roughly 4 hours, even with the help of some great blog guides. So I thought I would give it a go at simplyifying the process even further.


__1. The first step is to find your operating system in order to download the necessary components.__ I was using a Mac OS X Intel. Those options can be found <a href="http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html">here </a>. 
<br>

__2. Download Instant Client Basic, SDK and SQL*Plus.__ SDK stands for software development kit, and is a set of developer tools. SQLPlus is a command line tool for making SQL queries to an Oracle database. Those options can be found <a href="http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html">here</a>. One thing I will make note of, I originally tried downloading the latest Version, 11.2.0.3.0, but ended up having to remove all those directories and start over again with Version 10.2.0.4 with the 64-bit package.
<br>

__3. When I downloaded the instant client, SDK and SQLPlus, they were already unzipped,__ so I was able to bypass that step (I was using OS X, 10.8.5). The next step was to make a directory to store instant client: `mkdir ~/opt/oracle`
<br>

__4. Because instant client, SQL*Plus and SDK all download into different directories,__ I combined the SQLPlus and SDK directories into the instant client directory, making everything much more compact. 
<br>

__5. Next, I moved my instantclient directory into my `/opt/oracle` directory:__ `mv instantclient /opt/oracle`
<br>

__6. Setting up sym links to the libraries:__
      ln -s libclntsh.dylib.10.1 libclntsh.dylib
      ln -s libocci.dylib.10.1 libocci.dylib
<br>

__7. The next step is to set up `ORACLE_HOME`, `NLS_LANG` and `DYLD_LIBRARY_PATH` variables.__ I chose to set up my variables inside my .bash_profile:

      export ORACLE_HOME=/opt/oracle
      #export ORACLE_HOME=/usr/local/oracle
      export DYLD_LIBRARY_PATH=$ORACLE_HOME/instantclient
      export LD_LIBRARY_PATH=$DYLD_LIBRARY_PATH
      export SQLPATH=$ORACLE_HOME/instantclient
      export ARCHFLAGS="-arch x86_64"
      export NLS_LANG="AMERICAN_AMERICA.UTF8"
      export TNS_ADMIN=$ORACLE_HOME/network/admin
      export PATH=$PATH:$MAVEN_HOME/bin:$SVN_HOME/bin:$GIT_HOME/bin:$AMQ_PATH/bin:$DYLD_LIBRARY_PATH

Obviously, these paths will change base on your own selection for directory names.
<br>

__8. The final step is to put SQL*Plus in your $PATH.__ I did this by creating a symlink (I already had usr/local/bin in my $PATH)
      ln -s /opt/oracle/instantclient/sqlplus /usr/local/bin/sqlplus
You can test to make sure this works by running `which sqlplus`, which should return `/usr/local/bin/sqlplus`

__9. The final step is to test it all out! Try running `gem install ruby-oci8`__ and Oracle instant client should be all set up!


<ul>__Helpful Links:__
  <li>http://ruby-oci8.rubyforge.org/en/file.install-instant-client.html</li>
  <li>http://www.pixellatedvisions.com/2009/03/25/rails-on-oracle-part-1-installing-the-oracle-instant-client-on-mac-os-x</li>
</ul>
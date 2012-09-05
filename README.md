
[Based on David Ghedini's "Install Tomcat 7 on CentOS, RHEL, or Fedora" blogpost.](http://www.davidghedini.com/pg/entry/install_tomcat_7_on_centos)

## Install Djatoka Image Server and Viewer on RHEL : Michael Weigle  
###### Tuesday Aug 28, 2012  

This tutorial will cover installing and basic configuration of:  

 * Tomcat 7.0.29  
 * Djatoka Image Server 1.1  
 * Djatoka Viewer 2.1  

This tutorial assumes that you already have a JDK installed and that your JAVA_HOME points to it. If you are using a different release of the programs above, simply change the file names below accordingly.  

**Step 1: Download and verify the files**  

Download and unpack `BML-Djatoka-20120813.tar.gz` to `/tmp` using `tar -xzf`:  

    [root@server ~]# cd /tmp
    [root@server tmp]# tar -xzf BML-Djatoka-20120813.tar.gz

Now verify that you have the correct files (or updated versions of them):  

    [root@server tmp]# ls
      adore-djatoka-1.1.tar.gz            etc-scripts.tar.gz
      adore-djatoka-viewer-2.1.tar.gz     jdk-7u5-linux-x64.tar.gz
      apache-tomcat-7.0.29.tar.gz         



**Step 1: Install JDK 1.7.0_05**  

We will install Java 7 under `/usr/java`.  

Start by creating a new directory `/usr/java`:  

    [root@server tmp]# mkdir /usr/java

Change to the `/usr/java` directory we created:  

    [root@server tmp]# cd /usr/java
    [root@server java]#

Unpack `jdk-7u5-linux-x64.tar.gz` in the `java` directory using `tar -xzf`:  

    [root@server java]# tar -xzf /tmp/jdk-7u5-linux-x64.tar.gz

This will create the directory `/usr/java/jdk1.7.0_05`. This will be our `JAVA_HOME`.  



**Step 2: Install Tomcat 7.0.29**  

We will install Tomcat 7 under `/usr/share`.  

Switch to the `/usr/share` directory:  

    [root@server java]# cd /usr/share
    [root@server share]#

Unpack `apache-tomcat-7.0.29.tar.gz` in the `share` directory using `tar -xzf`:  

    [root@server share]# tar -xzf /tmp/apache-tomcat-7.0.29.tar.gz

This will create the directory `/usr/share/apache-tomcat-7.0.29`. This will be our `CATALINA_HOME`.  


*Step 2a: Adjust Tomcat Ports (Optional)*  

The default Tomcat installation uses ports 8080, 8443, and 8009. You can change these port numbers in `/usr/share/apache-tomcat-7.0.29/conf/server.xml`. Restart Tomcat to implement changes.


**Step 3: Install Adore Djatoka 1.1**  

We will install Djatoka 1.1 under `/usr/share` too.  

Unpack `adore-djatoka-1.1.tar.gz` in the `share` directory using 'tar -xzf':  

    [root@server share]# tar -xzf /tmp/adore-djatoka-1.1.tar.gz

This will create the directory `/usr/share/adore-djatoka-1.1`. This will be our `DJATOKA_HOME`.  

Copy `adore-djatoka.war` to the Tomcat installation:  

    [root@server share]# cp adore-djatoka-1.1/dist/adore-djatoka.war apache-tomcat-7.0.29/webapps



**Step 4: Configure Environment Variables and Tomcat to Run as a Service**  

These scripts will set `JAVA_HOME`, `CATALINA_HOME`, and `DJATOKA_HOME` on after logging in, and will let us run Tomcat using `/etc/init.d/tomcat start|stop|restart`.  

Switch to the `/etc` directory:  

    [root@server share]# cd /etc
    [root@server etc]#

Unpack `etc-scripts.tar.gz` in the `etc` directory using 'tar -xzf':  

    [root@server etc]# tar -xzf /tmp/etc-scripts.tar.gz

Switch to the `/etc/init.d` directory:  

    [root@server etc]# cd /init.d
    [root@server init.d]#

Make `tomcat` executable and register it with the `chkconfig` utility:  

    [root@server init.d]# chmod 755 tomcat
    [root@server init.d]# chkconfig --add tomcat
    [root@server init.d]# chkconfig --level 234 tomcat on

Switch to the `/etc/profile.d` directory:  

    [root@server init.d]# cd ../profile.d
    [root@server profile.d]#

Make `catalina.sh`, `djatoka.sh`, and `java.sh` executable:  

    [root@server profile.d]# chmod 755 catalina.sh djatoka.sh java.sh

Restart your session by logging in and out so that the scripts can run once.  



**Step 5: Verify Java, Djatoka, and Tomcat Installation.**  

Tomcat's startup script runs through Djatoka, so checking to make sure that Tomcat is working will also verify Djatoka and Java.  

    [root@server ~]# /etc/init.d/tomcat start
    Using CATALINA_BASE:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_HOME:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_TMPDIR: /usr/share/apache-tomcat-7.0.29/temp
    Using JRE_HOME:        /usr/java/jdk1.7.0_05
    Using CLASSPATH:       /usr/share/apache-tomcat-7.0.29/bin/bootstrap.jar:/usr/share/apache-tomcat-7.0.29/bin/tomcat-juli.jar

    [root@server ~]# /etc/init.d/tomcat stop
    Using CATALINA_BASE:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_HOME:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_TMPDIR: /usr/share/apache-tomcat-7.0.29/temp
    Using JRE_HOME:        /usr/java/jdk1.7.0_05
    Using CLASSPATH:       /usr/share/apache-tomcat-7.0.29/bin/bootstrap.jar:/usr/share/apache-tomcat-7.0.29/bin/tomcat-juli.jar

    [root@server ~]# /etc/init.d/tomcat start
    Using CATALINA_BASE:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_HOME:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_TMPDIR: /usr/share/apache-tomcat-7.0.29/temp
    Using JRE_HOME:        /usr/java/jdk1.7.0_05
    Using CLASSPATH:       /usr/share/apache-tomcat-7.0.29/bin/bootstrap.jar:/usr/share/apache-tomcat-7.0.29/bin/tomcat-juli.jar

    [root@server ~]# /etc/init.d/tomcat restart
    Using CATALINA_BASE:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_HOME:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_TMPDIR: /usr/share/apache-tomcat-7.0.29/temp
    Using JRE_HOME:        /usr/java/jdk1.7.0_05
    Using CLASSPATH:       /usr/share/apache-tomcat-7.0.29/bin/bootstrap.jar:/usr/share/apache-tomcat-7.0.29/bin/tomcat-juli.jar
    Using CATALINA_BASE:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_HOME:   /usr/share/apache-tomcat-7.0.29
    Using CATALINA_TMPDIR: /usr/share/apache-tomcat-7.0.29/temp
    Using JRE_HOME:        /usr/java/jdk1.7.0_05
    Using CLASSPATH:       /usr/share/apache-tomcat-7.0.29/bin/bootstrap.jar:/usr/share/apache-tomcat-7.0.29/bin/tomcat-juli.jar

We should review the Catalina.out log located at /usr/share/apache-tomcat-7.0.29/logs/catalina.out and check for any errors.  

    [root@server ~]# less /usr/share/apache-tomcat-7.0.29/logs/catalina.out



**Step 6: Install Adore Djatoka Viewer 2.1**  

Switch to the `CATALINA_HOME/webapps` directory:  

    [root@server share]# cd $CATALINA_HOME/webapps
    [root@server webapps]#

Unpack `adore-djatoka-viewer-2.1.tar.gz` in the `webapps` directory using 'tar -xzf':  

    [root@server webapps]# tar -xzf /tmp/BML-Djatoka-20120813/adore-djatoka-viewer-2.1.tar.gz

Adjust server variable in `CATALINA_HOME/webapps/adore-djatoka-viewer-2.1/viewer.html` to the public address of the tomcat server (for rocket it was rocket.richmond.edu:8081).
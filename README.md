[Based on David Ghedini's "Install Tomcat 7 on CentOS, RHEL, or Fedora" blogpost.](http://www.davidghedini.com/pg/entry/install_tomcat_7_on_centos)

## Install Djatoka Image Server and Viewer on RHEL : Michael Weigle  
###### Tuesday Sep 25, 2012  

This tutorial will cover installing and basic configuration of:  

 * Tomcat 7.0.29  
 * Djatoka Image Server 1.1  
 * Djatoka Viewer 2.1  

This tutorial assumes that you already have a JDK installed and that your JAVA_HOME points to it. If you are using a different release of the programs above, simply change the file names below accordingly.  
This tutorial also assumes that you already have a working installation of git. If not, please install one from http://git-scm.com/

**Step 1: Clone the repository**  

Clone repository to `/usr/share`: (you can clone it to another directory if you so choose)  

    [root@server ~]# cd /usr/share
    [root@server share]# git clone git://github.com/MetricMike/djatoka-richmond.git

This should give you Tomcat, Djatoka, and the Djatoka Viewer

*Step 2a: Adjust Tomcat's Directory Pointer (Optional)*

The default installation of this repository assumes that you have cloned it to /usr/share.
If this is not true, you need to change the `DIR` variable in the `tomcat` script.  

*Step 2b: Adjust Tomcat Ports (Optional)*  

The default Tomcat installation uses ports 8080, 8443, and 8009.
You can change these port numbers in `/usr/share/apache-tomcat-7.0.29/conf/server.xml`

*Step 2c: Adjust Djatoka Settings (Optional)*

The default Djatoka assumes connectivity from `localhost:8080`.
You can change this by adjusting the server variable in `/usr/share/djatoka/viewer.html`

*Step 2d: Adjust Permissions*

GitHub is not respecting permissions (or Windows reset them all), so you'll have to go through and change them manually.
THIS IS BAD PRACTICE, but the following will "fix" permissions problem.

    [root@server djatoka-richmond]# chmod -R 755 *.*

**Step 3: Configure Tomcat to Run as a Service**  

This will let Tomcat run using `/etc/init.d/tomcat start|stop|restart`.

Switch to the `/etc/init.d` directory:  

    [root@server etc]# cd /init.d
    [root@server init.d]#

Make `tomcat` executable and register it with the `chkconfig` utility:  

    [root@server init.d]# chmod 755 tomcat
    [root@server init.d]# chkconfig --add tomcat
    [root@server init.d]# chkconfig --level 234 tomcat on

Restart your session by logging in and out so that the scripts can run once.  

**Step 4: Verify Java, Djatoka, and Tomcat Installation.**  

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

We should review the Catalina.out log located at `/usr/share/djatoka-richmond/apache-tomcat-7.0.29/logs/catalina.out` and check for any errors.  
    
Finally, check `localhost:8080/adore-djatoka/index.html` to test Djatoka's functionality.
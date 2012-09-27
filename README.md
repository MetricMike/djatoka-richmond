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

**Step 2: Run the configuration script**

Execute the ./config script and follow the prompts.

Restart your session by logging in and out to ensure that tomcat gets registered and starts correctly.  

**Step 3: Verify Java, Djatoka, and Tomcat Installation.**  

Check `localhost:8080/adore-djatoka/index.html` to test Djatoka's functionality.

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

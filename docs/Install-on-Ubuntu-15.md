### Outdated Instruction

You can find the instruction of Ubuntu installation for previous version: [version 1.3 and 1.4 installation](Install-on-Ubuntu.md)

### Configure SSL

If you would like to secure your BIMserver installation with HTTPS, follow the [SSL Setup Guide](SSL-setup.md) for detailed instructions on generating and configuring an SSL certificate.

---

This guide explains how to install BIMserver on a Linux server with Ubuntu using Tomcat as application server. Root privileges are assumed. When running as a normal user, prepend `sudo` to commands that require elevated privilege. It has been tested in 11/2024 with BIMserver 1.5.185, Ubuntu 22.04.5 and Tomcat 9.0.95. Some sections contain a "Proxy note" in the end. Skip these if your server does not have to go through a proxy and jump to the next section.

## Basic Ubuntu configuration

First, configure and update the operating system if necessary.

```bash
user@local:~$ apt-get update                             # update package list
user@local:~$ apt-get upgrade                            # update installed packages to latest version
user@local:~$ apt-get install wget unzip default-jre     # install necessary packages
```

### Proxy note

If you are using proxy, add your proxy configuration to `/etc/profile` to configure proxy for terminal user, e.g. enable download with wget. This may not be necessary, if proxies are already configured. If you can run `wget` without this, omit this step.

```bash
user@local:~$ vim /etc/profile     # add the proxy settings (see below)
user@local:~$ logout               # logout and login again for the settings to take effect
```

Additional content for /etc/profile:

```sh
export http_proxy=http://[PROXY HOST]:[PORT]
export https_proxy=https://[PROXY HOST]:[PORT]
export no_proxy= [Use the proxy server except for addresses that start with the following entries]
export HTTP_PROXY=$http_proxy
export HTTPS_PROXY=$https_proxy
export NO_PROXY=$no_proxy
```

## Directories and user setup

We will create home directories for BIMserver and Tomcat in the `/var` directory and assigned the ownership of those directories to the tomcat user and group created for that purpose.

```sh
user@local:~$ useradd -s /sbin/nologin tomcat         # dedicated user for tomcat without shell access
user@local:~$ mkdir /var/bimserver/home               # create bimserver home (
user@local:~$ mkdir /var/tomcat                       # create Tomcat home directory for the Tomcat user
user@local:~$ chown -R tomcat:tomcat /var/bimserver/  # assign ownership to tomcat user and group
user@local:~$ chown -R tomcat:tomcat /var/tomcat/     # assign ownership to tomcat user and group
user@local:~$ vim /etc/passwd                         # edit to set Tomcat home (see below)
```

Content of /etc/passwd, user ID (1009) may vary:

```passwd
tomcat:x:1009:1009::/var/tomcat:/sbin/nologin
```

The home directory names and locations can be selected freely.
If you want to have multiple BIMserver home later, you can create a new home directory in `/var/bimserver`.

## Install Tomcat

```sh
user@local:~$ wget [link of tomcat.zip]             # Download tomcat (Make sure you replace this with the latest release!)
user@local:~$ mv apache-tomcat-9.0.93.zip /opt/     # move to opt folder (folder for additional software installations)
user@local:/opt$ unzip apache-tomcat-9.0.93.zip     # unzip Tomcat
user@local:/opt$ rm apache-tomcat-9.0.93.zip        # remove zip, keep exploded folder
user@local:/opt$ mv apache-tomcat-9.0.93 tomcat9    # rename folder
user@local:/opt$ chmod u+x tomcat9/bin/*.sh         # make scripts executable
user@local:/opt$ chown -R tomcat:tomcat tomcat9     # assign ownership to tomcat user and group
```

Remove unnecessary directories and contained files:

```sh
user@local:/opt/tomcat9/webapps$ rm -r ROOT manager host-manager docs examples
```

Start and stop tomcat:

```sh
user@local:~$ sudo -u tomcat /opt/tomcat9/bin/startup.sh   # start tomcat under dedicated user
user@local:~$ sudo /opt/tomcat9/bin/shutdown.sh            # stop tomcat
```

## Deploy and configure BIMserver

Although Tomcat has hot deployment enabled by default, it may be more safe to stop Tomcat before any modifications of the setup and restart after.

First download the BIMserver war and deploy it in Tomcat.

```sh
user@local:~$ wget [link of BIMserver.war]                                       # Download BIMserver
user@local:~$ mv bimserverwar-1.5.185.war /opt/tomcat9/webapps/bimserver.war     # move to webapps folder
```

Configuration

```sh
user@local:~$ mkdir /opt/tomcat9/conf/Catalina/localhost               # might already exist
user@local:~$ vim /opt/tomcat9/conf/Catalina/localhost/bimserver.xml   # configure bimserver homedir (see below)
```

Content of `bimserver.xml` (enter your bimserver [home](#directories-and-user-setup) as value for the "homedir" parameter):

```xml
<Context>
  <Parameter name="homedir" value="/var/bimserver/home"/>
</Context>
```

### Proxy note

If you are using proxy, configure the proxy settings in `setenv.sh`. This step may still be necessary, even if the proxy server is already configured externally for the terminal user. You can first attempt to setup BIMserver to check if this configuration is required. If plugin installation fails, delete the home folder and start over again.

```sh
user@local:~$ vim /opt/tomcat9/bin/setenv.sh     # enter proxy settings for Tomcat
```

Additional content of `setenv.sh`:

```sh
CATALINA_OPTS="-Dhttp.proxyHost=[PROXY HOST] -Dhttp.proxyPort=[PORT] -Dhttps.proxyHost=[PROXY HOST] -Dhttps.proxyPort=[PORT]"
```

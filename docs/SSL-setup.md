When running as a normal user, prepend `sudo` to the commands where elevated permissions are needed.

## Generate the SSL Certificate

We will use [Certbot ](https://certbot.eff.org/) to create an SSL certificate. Certbot is a free, easy-to-use tool that works with [Let's Encrypt](https://letsencrypt.org/), a widely trusted certificate authority, to issue the certificate.

The domain `opensourcebim.org` is used as an example below. The certbot will use the domain name for directory names. Replace it with your domain name when executing the commands.

```sh
user@local:~$ man snap                           # check if snap is installed 
user@local:~$ snap install --classic certbot     # install certbot
user@local:~$ certbot certonly --standalone      # generate SSL certificate (interactively enter required information)
user@local:~$ systemctl list-timers              # check automatic renewal
user@local:~$ chgrp tomcat /etc/letsencrypt/live/opensourcebim.org/privkey.pem  # own private key by group tomcat
user@local:~$ chmod g+r /etc/letsencrypt/live/opensourcebim.org/privkey.pem     # allow group to read private key
```

By default, read-access to the private key is restricted to root only which is not sufficient for Tomcat, as it does not allow for a dedicated startup phase with elevated permissions for sensible operations. Certbot picks up any modified permissions upon renewal.


## Configuration

Configure SSL in `server.xml`:
```xml
<Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
           maxThreads="150" SSLEnabled="true"
           maxParameterCount="1000">
    <SSLHostConfig>
        <Certificate certificateKeyFile="/etc/letsencrypt/live/opensourcebim.org/privkey.pem"
                     certificateFile="/etc/letsencrypt/live/opensourcebim.org/cert.pem"
                     certificateChainFile="/etc/letsencrypt/live/opensourcebim.org/chain.pem"
                     type="EC" />
    </SSLHostConfig> 
</Connector> 
```

Force redirection to SSL via configuration in `web.xml`:
```xml
<security-constraint>
     <web-resource-collection>
         <web-resource-name>Protected Context</web-resource-name>
         <url-pattern>/*</url-pattern>
     </web-resource-collection>
     <user-data-constraint>
         <transport-guarantee>CONFIDENTIAL</transport-guarantee>
     </user-data-constraint>
</security-constraint> 
```
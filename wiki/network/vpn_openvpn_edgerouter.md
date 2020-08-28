# OpenVPN on EdgeRouter

This will describe the basic steps and commands to create an OpenVPN server on EdgeRouter and set up a client connection.
All this information is avaliable in several locations (like the [EdgeRouter documentation](https://help.ui.com/hc/en-us/articles/115015971688-EdgeRouter-OpenVPN-Server) or [other pages](http://www.forshee.me/2016/03/16/ubiquiti-edgerouter-lite-setup-part-5-openvpn-setup.html)), but as it usually happens with EdgeRouter procedures, I had to tinker with it a bit to make it work.

This setup considers the following scenario:
* An EdgeRouter, with a clean config, set up using the Basic Wizard (tested with an ERX-SFP and an ER-LITE3)
* A DNS record for the WAN interface IP (tested with a Cloudflare DNS for a dynamic IP [as shown in this wiki](https://hfiel.github.io/wiki/network/edgerouter_cloudflare_dyndns.html))  
* The local subnet address range in the example is 10.10.1.0/24
* The internal IP of the EdgeRouter is 10.10.1.1
* The VPN subnet address range is 10.100.1.0/24
* You have basic understanding of networking, SSH and SCP, and command line usage

This is a diagram of the scenario:

![OpenVPN server on EdgeRouter](https://github.com/hfiel/hfiel.github.io/blob/master/wiki/network/openvpn_edgerouter.png "OpenVPN server on EdgeRouter")

To begin, connect to the EdgeRouter by SSH:

```ssh ubnt@10.10.1.1```

## Creating the CA, keys and certificates:

This part of the configuration must be done as the root user:

```sudo su```

We first will create a Diffie-Hellman parameter file (this takes several minutes on the router, please be patient)

```openssl dhparam -out /config/auth/dh.pem -2 2048```

Change into the folder '/usr/lib/ssl/misc':

```cd /usr/lib/ssl/misc```

We then create a root CA keys using the following script:

```./CA.pl -newca```

This script will request several details, including a password for the CA (please make good note of it). Fill them with your values, here is an exmple of the output (do not copy these lines): 

```
CA certificate filename (or enter to create)

Making CA certificate ...
Generating a 2048 bit RSA private key
...............................................................+++
......................................................................+++
writing new private key to './demoCA/private/cakey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Madrid
Locality Name (eg, city) []:Madrid
Organization Name (eg, company) [Internet Widgits Pty Ltd]:hfiel.es
Organizational Unit Name (eg, section) []:Certificate Authority
Common Name (e.g. server FQDN or YOUR name) []:root
Email Address []:ca@hfiel.es

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:hfiel
Using configuration from /usr/lib/ssl/openssl.cnf
Enter pass phrase for ./demoCA/private/cakey.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number:
            ab:cd:ef:01:23:45:67:89
        Validity
            Not Before: Aug 01 12:13:14 2020 GMT
            Not After : Aug 01 12:13:14 2021 GMT
        Subject:
            countryName               = ES
            stateOrProvinceName       = Madrid
            organizationName          = hfiel.es
            organizationalUnitName    = Certificate Authority
            commonName                = root
            emailAddress              = ca@hfiel.es
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                AB:CD:EF:A0:A1:A2:A3:B0:B1:B2:B3:00:11:22:33:44:55:66:77:88
            X509v3 Authority Key Identifier: 
                keyid:AB:CD:EF:A0:A1:A2:A3:B0:B1:B2:B3:00:11:22:33:44:55:66:77:88

            X509v3 Basic Constraints: 
                CA:TRUE
Certificate is to be certified until Aug 01 12:13:14 2020 GMT (365 days)

Write out database with 1 new entries
```

Now copy the generated cert and key files into 'config/auth': 
```
cp demoCA/cacert.pem /config/auth
cp demoCA/private/cakey.pem /config/auth
```

 - NOTE: if you have already followed this procedure before, you will already have a *demoCA* folder. You may need to delete it to perform this step again. 
 
 ## Create the Server certificates
 
Now that we have the CA, we will create and sign the certificate for the server, again using scripts.

First, generate a new certificate request:

```./CA.pl -newreq```

Fill the details, and you will be asked for a PEM pass phrase for the certificate. Please make note of it.
(Again, this is an output example, do not copy these lines)

```
writing new private key to 'newkey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Madrid
Locality Name (eg, city) []:Madrid
Organization Name (eg, company) [Internet Widgits Pty Ltd]:hfiel.es
Organizational Unit Name (eg, section) []:My VPN Server Name
Common Name (e.g. server FQDN or YOUR name) []:myvpn.hfiel.es
Email Address []:vpn@hfiel.es

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:hfiel
Request is in newreq.pem, private key is in newkey.pem
```

Now we will sign this request with the CA keys. 
```
./CA.pl -sign
```

Verify the details and answer the questions. You will need the CA password.
(Again, this is an output example, do not copy these lines)

```
Using configuration from /usr/lib/ssl/openssl.cnf
Enter pass phrase for ./demoCA/private/cakey.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number:
            ab:cd:ef:01:23:45:67:89
        Validity
            Not Before: Aug 01 12:13:14 2020 GMT
            Not After : Aug 01 12:13:14 2021 GMT
        Subject:
            countryName               = ES
            stateOrProvinceName       = Madrid
            organizationName          = hfiel.es
            organizationalUnitName    = My VPN Server Name
            commonName                = myvpn.hfiel.es
            emailAddress              = vpn@hfiel.es
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
          X509v3 Subject Key Identifier: 
                AB:CD:EF:A0:A1:A2:A3:B0:B1:B2:B3:00:11:22:33:44:55:66:77:88
            X509v3 Authority Key Identifier: 
                keyid:AB:CD:EF:A0:A1:A2:A3:B0:B1:B2:B3:00:11:22:33:44:55:66:77:88

Certificate is to be certified until Aug 01 12:13:14 2020 GMT (365 days)

Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
Signed certificate is in newcert.pem
```
 
Move the files to the */config/auth* location:

```
mv newcert.pem /config/auth/server.pem
mv newkey.pem /config/auth/server.key
```

Remove the pasword for the server KEY file (if you do not remove it, start of the server will always fail waiting for you to insert the password):

```
openssl rsa -in /config/auth/server.key -out /config/auth/server-no-pass.key
mv /config/auth/server-no-pass.key /config/auth/server.key
```

Finally, we will generate a TLS AUTH KEY (to enhance the security of the connections to the server)

```
openvpn --genkey --secret ta.key  
mv ta.key /config/auth/ta.key
```


 ## Create the Client certificate

Here we will generate the certificate for ONE client (*client_hfiel* in the example). You can repeat this process to create several clients if needed.
The process is the same as with the server: first generate a certificate, and the sign it, so this time I will avoid the output.


Generate the certificate (you will be asked to set a password, plase make good note of it)
```
./CA.pl -newreq
```

Sign the certificate (you will be asked to provide the CA password)

```
./CA.pl -sign
```

Now move the certificate PEM and KEY files to the */config/auth* location:

```
mv newcert.pem /config/auth/client_hfiel.pem
mv newkey.pem /config/auth/client_hfiel.key
```

**OPTIONALLY** you can remove the password from the client certificate. If you do this, the OpenVPN client will not request a password when connecting.
Please note that will ease the setting for the clients, but may reduce the overall security.

```
openssl rsa -in /config/auth/client_hfiel.key -out /config/auth/client_hfiel-no-pass.key
mv /config/auth/client_hfiel-no-pass.key /config/auth/client_hfiel.key
```

Finally, set the proper permissions for the key file:

```
chmod 644 /config/auth/client_hfiel.key
```

# Exit the ROOT user

This is important: the main preparation part (certificates) which is done as the root user is completed, and now before moving to the configuration we must exit the root user.

```
exit
```

# Configure the OpenVPN server on the Edgerouter

We will now configure the OpenVPN server in three steps. All this is performed as the normal user (not root) in the configuration mode, so first we need to enter into this mode:

```
configure
```

## Firewall rules

set the firewall rules to allow the connection. The default port for OpenVPN is 1194, but I will set this server in 1195 as an example.

```
set firewall name WAN_LOCAL rule 30 action accept
set firewall name WAN_LOCAL rule 30 description openvpn
set firewall name WAN_LOCAL rule 30 destination port 1195
set firewall name WAN_LOCAL rule 30 protocol udp
```

## VPN Server: network settings

Now we will create and define the OpenVPN server settings with regards to the network. Note the diagram from the beggining to clarify each subnet and adapt to your specific configuration.
Remember that here:
* 10.**10**.1.0/24 is the local subnet (note the 10).
* 10.**10**.1.1 is the EdgeRouter IP address in the local subnet. In my configuration I use the router itself and the DNS server for the OpenVPN connection
* 10.**100**.1.0/24 is the VPN subnet (note the 100). The VPN clients will get an IP of this subnet. This subnet **must** be different from your local subnet (and also different from any other subnet that you may have).
* 1195 is the port to use for the service. Remember that the default port is 1194, but I like to change it to avoid some basic automatic port scans.

A personal recommendation: do yourself a favor and do NOT use 192.168.1.0/24 for the local or VPN subnet ranges: this is the default subnet for many home devices, and using it will cause you troubles trying to connect from them.

```
set interfaces openvpn vtun0 mode server
set interfaces openvpn vtun0 server subnet 10.100.1.0/24
set interfaces openvpn vtun0 server push-route 10.10.1.0/24
set interfaces openvpn vtun0 server name-server 10.10.1.1
set interfaces openvpn vtun0 openvpn-option "--port 1195"
```

## VPN Server: additional and security settings

Now we will define some additional settings, including some to enhance the security:

```
set interfaces openvpn vtun0 openvpn-option --tls-server
set interfaces openvpn vtun0 openvpn-option "--tls-auth /config/auth/ta.key 0"
set interfaces openvpn vtun0 openvpn-option "--comp-lzo yes"
set interfaces openvpn vtun0 openvpn-option --persist-key
set interfaces openvpn vtun0 openvpn-option --persist-tun
set interfaces openvpn vtun0 openvpn-option "--keepalive 10 120"
set interfaces openvpn vtun0 openvpn-option "--user nobody"
set interfaces openvpn vtun0 openvpn-option "--group nogroup”
```

## Save the configuration

Once all the settings are ready, we will commit and save the changes. The server will start inmediately.

```
commit
save
```

If you get an error after commit, there must be some kind of typo or mistake in your settings. Please double verify each command.

When you can not find the error, you can discard the changes:

```
discard
```

and then run each "set" command and after it do a "commit". If you get an error, that previous set command as your mistake.


## Exit the configuration mode

Once all the settings are applied successfully, exit the configuration mode:

```
exit
```


# Prepare the files for the client connections

All is ready, but now for each client you will need to prepare a set of files with the certificates and settings for the OpenVPN client.
Here I will show a generic config that I have tested on both Android phones and Ubuntu 20.04 using the official OpenVPN client. You may need to adjust other settings (both in this file, and also maybe on the server itself) for other systems and requirements. 

You now need to perform two steps:
1) Copy some files from the router to your computer
2) Create an OpenVPN config file for the client.

## Copy the client certificates
You will need to copy four files from the router: you can do this with SCP using your favourite method (command line, file browser in linuxm WinSCP...). I will not enter in details here.

Copy the following files:

|      File     | Location on EdgerRouter |
| ------------- | ---------------------- |
| cacert.pem | */config/auth/cacert.pem* |
| ta.key | */config/auth/ta.key* |
| client_hfiel.key | */config/auth/client_hfiel.key* |
| client_hfiel.pem | */config/auth/client_hfiel.pem* |

Two of the files (*cert.pem* and *ta.key*) are common for all the clients, and the *client_name* KEY and PEM files will be the ones changing for each different user.

*NOTE*: if you have troubles copying some file (due to permissions), please DO NOT change the permissions of the original files (as that may cause a security issue). I recommend that you copy the files to a temporary location (*/tmp*), change the permissions there, and once the files are copied to your computer remove the temporary copy. 

## Generate the client configuration file

Besides the four files, you will need a *configuration.ovpn* file. I will show you here a generic example, and will mark in bold which parameters you must adjust, and explain those values.
This file must be sotred in a folder algonside the other 4 certificate files. Once you load this configuration file in the OpenVPN client you should be able to connect to your server.

<pre>
client
dev tun
proto udp
cipher AES-256-CBC
auth SHA256
# here you must define the DNS name of your server (or IP address), and the port for the OpenVPN server defined in the firewall and the router configuration
remote <b>myvpn.hfiel.es 1195</b>
resolv-retry infinite 
nobind
persist-key 
persist-tun 
verb 3
redirect-gateway def1
comp-lzo yes
user nobody
group nogroup
ca cacert.pem 
# Here you must specify the name of the client PEM and KEY files
cert <b>client_hfiel.pem</b>
key <b>client_hfiel.key</b>
tls-auth ta.key 1
</pre>

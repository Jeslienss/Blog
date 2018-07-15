Apache Server Configuretion
===========================

You first need to install the apache over the server.  
System: `UBUNTU 16.04.3`  
Install Apache: `sudo apt-get install apache2 `  

Create the server certificate
-----------------------------
 - Create the structure directory and protect from other users:
    ```sh
    mkdir ssl
    chmod 0700 ssl
    cd ssl
    mkdir certs private
    ```
 - Create a database to keep track of each certificate signed:
    ```sh
    echo '100001' > serial
    $touch certindex.txt
    ```
 - Make a custom config file for openssl to use (Attached in the end).
 - Create root certificate (CA):
    ```sh
    openssl req -new -x509 -extensions v3_ca -keyout private/cakey.pem -out cacert.pem -days 365 -config ./openssl.cnf
    ```
    Here, openssl requires challenge password, by which entity may request certificate revocation. The password of our CA is "1234".
    This script will create `private/cakey.pem` that is the private key of the CA and the `cacert.pem` that is the which is the one you can give to others for import in their browsers.
 - Create a key and signing request for the server:
 	```sh
 	openssl req -new -nodes -out server-req.pem -keyout private/server-key.pem -days 365 -config ./openssl.cnf
 	```
 	The output of this script is ```server-req.pem``` that is the CSR and the ```private/server-key.pem``` that is the private key
 - Sign the request:
 	```sh
 	openssl ca -out server-cert.pem -days 365 -config ./openssl.cnf -infiles server-req.pem
 	```
 	This will produce `server-cert.pem` that is the public certificate.

Create the client certificate and the PKCS12 container
------------------------------------------------------
 - Create a key and signing request for each client:
 	```sh
 	openssl req -new -nodes -out client-req.pem -keyout private/client-key.pem -days 365 -config ./openssl.cnf
 	```
 	`private\client-key.pem` is the private key, and `client-req.pem` is the client request.
 - Sign the request:
 	```sh
 	openssl ca -out name-cert.pem -days 365 -config ./openssl.cnf -infiles client-req.pem
 	```
 	`name-cert.pem` is the client certificate.
 - Create the PKCS12 file:
 	```sh
 	openssl pkcs12 -export -in name-cert.pem -inkey private/client-key.pem -certfile cacert.pem -name "lijiawei" -out name-cert.p12
 	```
 	The produced file `name-cert.p12` is the file that the server will require to comunicate, and you have to install on the client.

# Configure APACHE
(you need to modify the path according to your server)
 - First Step: Only open server certificate.
```
cd /etc/apache2/site-available
sudo vim default-ssl.conf
```
Modified config file looks like the following:
```
<IfModule mod_ssl.c>
    <VirtualHost _default_:443>
        ServerAdmin webmaster@localhost

        DocumentRoot /var/www/html

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        SSLEngine on

        # Location of certificate and key
        SSLCertificateFile  /home/jiawei/Desktop/ssl/server-cert.pem
        SSLCertificateKeyFile /home/jiawei/Desktop/ssl/private/server-key.pem
        
        #SSLCACertificateFile /home/jiawei/Desktop/ssl/cacert.pem

        #SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt
        #SSLCACertificatePath /etc/ssl/certs/
        
        #   Client Authentication (Type):
        #   Client certificate verification type and depth.  Types are
        #   none, optional, require and optional_no_ca.  Depth is a
        #   number which specifies how deeply to verify the certificate
        #   issuer chain before deciding the certificate is not valid.
        #SSLVerifyClient require
        #SSLVerifyDepth  10


        <FilesMatch "\.(cgi|shtml|phtml|php)$">
                SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
                SSLOptions +StdEnvVars
        </Directory>

    </VirtualHost>
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet

```

Then, you need to reload the apache service.
```
$ sudo a2ensite default-ssl.conf
$ sudo service apache2 reload
$ sudo service apaches restart
```

You must use HTTPS to visit the website (localhost).

 - Second step: Open client certificate.
Remove the comments of:
```
#SSLCACertificateFile /home/jiawei/Desktop/ssl/cacert.pem
#SSLCertificateChainFile /etc/apache2/ssl.crt/server-ca.crt
#SSLCACertificatePath /etc/ssl/certs/
```

Our website is located in `/var/www/html/sim-project`
The config files are located in `/etc/apache2/site-available` and `/etc/apache2/site-enabled`


# CA config file
```

dir = .

[ ca ]
default_ca = CA_default

[ CA_default ]
serial = $dir/serial
database = $dir/certindex.txt
new_certs_dir = $dir/certs
certificate = $dir/cacert.pem
private_key = $dir/private/cakey.pem
default_days = 365
default_md = md5
preserve = no
email_in_dn = no
nameopt = default_ca
certopt = default_ca
policy = policy_match

[ policy_match ]
countryName = match
stateOrProvinceName = match
organizationName = match
organizationalUnitName = optional
commonName = supplied
emailAddress = optional

[ req ]
default_bits = 2048 # Size of keys
default_keyfile = key.pem # name of generated keys
default_md = md5 # message digest algorithm
string_mask = nombstr # permitted characters
distinguished_name = req_distinguished_name
req_extensions = v3_req

[ req_distinguished_name ]
countryName = Country Name (2 letter code)
countryName_default = US
countryName_min = 2
countryName_max = 2

stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = MI

localityName = Locality Name (eg, city)
localityName_default = East Lansing

organizationName = Organization Name (eg, company)
organizationName_default = MSU

organizationalUnitName = Organizational Unit Name (eg, section)
organizationalUnitName_default = NSSLAB

commonName = Common Name (eg, YOUR name)
commonName_max = 64
commonName_default = www.nsslab.com

emailAddress = Email Address
emailAddress_max = 64

[ req_attributes ]
challengePassword = A challenge password
challengePassword_min = 4
challengePassword_max = 20

unstructuredName = An optional company name
[ v3_ca ]
basicConstraints = CA:TRUE
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer:always

[ v3_req ]
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
```

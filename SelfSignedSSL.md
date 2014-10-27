###Creating a self-signed SSL Certificate
Adapted from http://www.akadia.com/services/ssh_test_certificate.html

When developing from a local server, you can get setup pretty quickly with OpenSSL following the commands below.

#####Step 1: Generate a Private Key
```
openssl genrsa -des3 -out server.key 1024`
```
Output:
```
Generating RSA private key, 1024 bit long modulus
.........................................................++++++
........++++++
e is 65537 (0x10001)
Enter PEM pass phrase:
Verifying password - Enter PEM pass phrase:
```
#####Step 2: Generate a CSR (Certificate Signing Request)
```
openssl req -new -key server.key -out server.csr
```
Output:
```
Country Name (2 letter code) [GB]:CH
State or Province Name (full name) [Berkshire]:Bern
Locality Name (eg, city) [Newbury]:Oberdiessbach
Organization Name (eg, company) [My Company Ltd]:Akadia AG
Organizational Unit Name (eg, section) []:Information Technology
Common Name (eg, your name or your server's hostname) []:public.akadia.com
Email Address []:martin dot zahn at akadia dot ch
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```
#####Step 3: Remove Passphrase from Key
```
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key
```
#####Step 4: Generating a Self-Signed Certificate
To generate a temporary certificate which is good for 365 days, issue the following command:
```
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
Output
```
Signature ok
subject=/C=CH/ST=Bern/L=Oberdiessbach/O=Akadia AG/OU=Information
Technology/CN=public.akadia.com/Email=martin dot zahn at akadia dot ch
Getting Private key
```
#####Step 5: Installing the Private Key and Certificate
When Apache with mod_ssl is installed, it creates several directories in the Apache config directory. The location of this directory will differ depending on how Apache was compiled.
```
cp server.crt /usr/local/apache/conf/ssl.crt
cp server.key /usr/local/apache/conf/ssl.key
```
#####Step 6: Configuring SSL Enabled Virtual Hosts
```
NameVirtualHost *:80
NameVirtualHost *:443

<VirtualHost *:80> 
    DocumentRoot "C:\xampp\htdocs"
    ServerName localhost
</VirtualHost>

<VirtualHost *:80>
    DocumentRoot "C:\xampp\htdocs\CleanScript\danielalamo.com"
    ServerName dan.dev
  	<Directory "C:\xampp\htdocs\CleanScript\danielalamo.com">
	    Order allow,deny
	    Allow from all
  	</Directory>
</VirtualHost>

<VirtualHost *:443>
    DocumentRoot "C:\xampp\htdocs\wela_app"
    ServerName test.dev
    SSLEngine on
    SSLCertificateFile "conf/ssl.crt/server.crt"
    SSLCertificateKeyFile "conf/ssl.key/server.key"
    <Directory "C:\xampp\htdocs\wela_app">
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```
#####Step 7: Restart Apache and Test
```
/etc/init.d/httpd stop
/etc/init.d/httpd stop
```
Guide taken from https://public.akadia.com

# sslcertgen

## Step 1: Find your openssl.cnf file. It is likely located in /usr/lib/ssl/openssl.cnf:

$ find /usr/lib -name openssl.cnf
/usr/lib/openssl.cnf
/usr/lib/openssh/openssl.cnf
/usr/lib/ssl/openssl.cnf

On AWS EC2 Ubuntu, /usr/lib/ssl/openssl.cnf is used by the built-in openssl program

## Edit openss.conf

Keep backup and replace with openssl.conf; then make following changes -

- Under [ alternate_names ] -> add 
DNS.1= <ALB domain name>

- Under [ v3_ca ], add following lines:
subjectAltName      = @alternate_names
keyUsage = digitalSignature, keyEncipherment

- Uncoment this line:   copy_extensions = copy  

## Generate the certificate

Generate:

$ openssl genrsa -out private.key 2048   (AWS only takes 2048)

$ openssl req -new -x509 -key private.key -sha256 -out certificate.pem -days 730

certificate.pem and private.key files are generated

To examine cert
$ openssl x509 -in certificate.pem -text -noout

## Import certificate in AWS Certificate Manager

Import certificate in AWS Certificate Manager for using with ALB

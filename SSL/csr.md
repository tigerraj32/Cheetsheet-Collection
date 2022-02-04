# CSR (Certificate Signing Request)

A CSR (Certificate Signing Request) s one of the first steps towards getting your own `ssl certificate` that is generated in the machine where we want to install the ssl certificate. The CSR Certificate consist of some vital key information that will help to recognize the organization such as

- Common Name :( Usually Fully Qualified domain name, eg. domain.com, *.domain.com)
- Organization 
- Organization Unit
- City/Locality
- state/County/region
- Country
- email address

CSR also contains the `private / public key` pairs. SSL will use this `public key` to encrypt data during ssl session and `private key` will used to decrypt the data during ssl encryption.

## Create SCR certificate and Private key

```
openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr

```

Make sure to replace your_domain with the actual domain you’re generating a CSR for.
The commands are broken out as follows:

- openssl –> activates the OpenSSL software
- req –> indicates that we want a CSR
- new, newkey –> generate a new key
- rsa:2048 –> generate a 2048-bit RSA mathematical key
- nodes –> no DES, meaning do not encrypt the private key in a PKCS#12 file
- keyout –> indicates the domain you’re generating a key for
- out –> specifies the name of the file your CSR will be saved as


This will generate two files `server.csr` : CSR certificate and `server.key` : private key


## Generate self sign certificate (SSL Certificate)

```
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt

```
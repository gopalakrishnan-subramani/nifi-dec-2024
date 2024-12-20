# Quick Demo, SSL, Java Key chain, Trust Store


```
mkdir certs
```


###Keystore:
A keystore is a secure repository that stores private keys and their associated certificates. It is used for authentication and encryption, ensuring only trusted entities can access encrypted data or services.

###Truststore:
A truststore is a repository containing trusted certificates, typically public key certificates of Certificate Authorities (CAs). It is used to validate external certificates, ensuring secure communication by verifying the authenticity of peer entities.

####What key file contains:
A key file contains private keys, which are essential for decryption and signing operations. It may also include associated public keys or certificates but primarily focuses on securing the private cryptographic key.

####What truststore key contains:
A truststore contains public key certificates from trusted parties or CAs. These are used to verify the validity of certificates presented by others during secure communication, ensuring they come from reliable sources.

####When to use PEM:
PEM (Privacy-Enhanced Mail) format is used when sharing cryptographic keys or certificates in a base64-encoded ASCII text format. It is suitable for interoperability between systems or when human-readable formats are preferred.



 
```
keytool -genkeypair -alias quickdemo-key -keyalg RSA -keysize 2048 \
  -dname "CN=localhost, OU=Org, O=Training, L=Bengaluru, ST=KA, C=India" \
  -keystore certs/quickdemo-keystore.jks -storepass password -keypass password

```

for jks file
```
ls
```


for pem file
```
keytool -exportcert -alias quickdemo-key -keystore certs/quickdemo-keystore.jks  -file certs/quickdemo-cert.pem -storepass password
```



```
ls
```

```
keytool -importcert -file certs/quickdemo-cert.pem -alias quickdemo-cert -keystore certs/quickdemo-truststore.jks -storepass password -noprompt
```

for keystore file
```
ls
```


```
docker compose up
```

open browser with https://localhost:4443

Accept security risk, go to https://localhost:4443/nifi or wait for redirect..

```
username: nifi-admin

password: Ni123456789@

```
 
look for Top Right for the name. This is default user/login mode. However we will use ldap for nifi cluster


```
docker logs nifi | grep "username" 
docker logs nifi | grep "password" 
docker logs nifi | grep "authentication" 
docker logs nifi | grep "single-user-identity-provider" 
```


Step 1: Export the Certificate and Key from the Keystore

    Export the certificate and private key from the keystore.jks into a PKCS12 file:

```
    keytool -importkeystore -srckeystore ./certs/quickdemo-keystore.jks -destkeystore ./certs/quickdemo-keystore.p12 -deststoretype PKCS12
```

Step 2: Extract the Private Key

    Extract the private key from the keystore.p12 file using openssl:

```
openssl pkcs12 -in ./certs/quickdemo-keystore.p12  -nodes -nocerts -out ./certs/nifi-key.key
```


Step 3: Extract the Public Certificate (if Needed)

    Extract the public certificate from the keystore.p12:

```
    openssl pkcs12 -in certs/quickdemo-keystore.p12 -clcerts -nokeys -out ./certs/nifi-cert.pem
```
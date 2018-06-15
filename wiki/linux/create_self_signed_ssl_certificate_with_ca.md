# Creation of self signed SSL certificates with your own CA

Via: <https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309>

*Important*: this is only for testing. If you need a real certificate and don't want to pay for it, see <https://letsencrypt.org/>

To generate a self signed certificate with your own CA, with a duration of 10 years

1.- Generate the CA private key (without password):

```
 openssl genrsa -out certificate-ca.key 4096
```

2.- Generate the CA root certificate

```
 openssl req -x509 -new -nodes -key certificate-ca.key -sha256 -days 3650 -out certificate-ca.crt
```

3.- Generate the host private key

```
 openssl genrsa -out certificate.key 2048
```

4.- Generate the signing request (you will need to provide the full fqdn that you want to protect)

```
 openssl req -new -key certificate.key -out certificate.csr 
```

5.- Generate the host certificate using the CA root certificate

```
 openssl x509 -req -in certificate.csr -CA certificate-ca.crt -CAkey certificate-ca.key -CAcreateserial -out certificate.crt -days 3650 -sha256
```

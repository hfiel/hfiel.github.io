# Concatenation of SSL certificates for using with HAProxy

Vía:  [https://www.meshcloud.io/en/2017/04/18/pem-file-layout-for-haproxy/](https://www.meshcloud.io/en/2017/04/18/pem-file-layout-for-haproxy/)

* In order to use SSL certificates with HAProxy you must concatenate all the related certificate files into one single .pem file.

The order is important:

1. The Certificate for your domain
2. The intermediates in ascending order to the Root CA
3. A Root CA, if any (usually none)
4. Private Key

So for example if you have:
* Your certificate: certificate.crt
* Your CA certificates (intermediates): certificate-ca.crt
* Your key: certificate.key

you would need to concatenate:

```
 cat certificate.crt certificate-ca.crt certificate.key > certificate.pem
```

Keep in mind: sometimes when you concatenate the files the end and begin blocks of two certificates get merged together in one single line.

Please review your .pem file and make sure you don't have anything like:

```
 -----END PRIVATE KEY----------BEGIN CERTIFICATE-----
```

If you have it, please divide it in two lines (there are 5 dashes "-" in front and behind each line):

```
 -----END PRIVATE KEY-----
 -----BEGIN CERTIFICATE-----
```

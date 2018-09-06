# Verifying SSL certificates from terminal

* To verify SSL certificates from terminal we can use curl or openssl

With curl (without redirects): use the option v without the option L

```
curl -Iv http://google.com
```


With curl (following redirects): use both option v AND option L

```
curl -ILv http://google.com
```

With openssl:

```
openssl s_client -connect google.com:443
```

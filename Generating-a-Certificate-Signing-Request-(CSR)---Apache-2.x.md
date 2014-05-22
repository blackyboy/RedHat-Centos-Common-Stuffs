Generating a Certificate Signing Request (CSR) - Apache 2.x

```
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.key -out yourdomain.csr
```
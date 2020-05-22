# Example


```bash
openssl req -x509 -newkey rsa:512 -keyout key.der -keyform der \
-out cert.der -outform der
```

生成PEM格式key和cert：

```bash
openssl req -x509 -newkey rsa:512 -days 365 -keyout key.pem -out cert.pem \
-subj "/C=CN/ST=HeBei/L=BaoDing/O=Fruit Ltd/CN=www.fruit.com"
```

生成DER格式key和cert：

```bash
openssl req -x509 -newkey rsa:512 -days 365 \
-keyform DER -keyout key.der -outform DER -out cert.der \
-subj "/C=CN/ST=HeBei/L=BaoDing/O=Fruit Ltd/CN=www.fruit.com"
```


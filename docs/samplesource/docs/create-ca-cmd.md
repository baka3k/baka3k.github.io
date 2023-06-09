---
layout: default
title: Create CA by command
parent: 02. Sample Source Code
nav_order: 105
---

1. Create an auxiliary file “android_options.txt” with this line inside:
```
basicConstraints=CA:true
```
2. Create self-signed certificate by Execute below commands from CMD - one by one

```
openssl genrsa -out priv_and_pub.key 2048
```
```
openssl req -new -days 3650 -key priv_and_pub.key -out CA.pem
```
```
openssl x509 -req -days 3650 -in CA.pem -signkey priv_and_pub.key -extfile ./android_options.txt -out CA.crt
```

After that, you can find CA.crt & CA.pem in yout folder
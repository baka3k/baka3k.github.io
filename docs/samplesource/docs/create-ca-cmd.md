---
layout: default
title: Create CA by command
parent: 02. Sample Source Code
nav_order: 105
---

1. First, create an auxiliary file “android_options.txt” with this line inside:

```xml
basicConstraints=CA:true
```

2. Create self-signed certificate by Execute below commands from CMD - **one by one**

```xml
openssl genrsa -out priv_and_pub.key 2048
```

```xml
openssl req -new -days 3650 -key priv_and_pub.key -out CA.pem
```

```xml
openssl x509 -req -days 3650 -in CA.pem -signkey priv_and_pub.key -extfile ./android_options.txt -out CA.crt
```

After that, you can find `CA.crt` & `CA.pem` in yout folder

*In additions, you can convert cert to DER format by command* 
```xml
openssl x509 -inform PEM -outform DER -in CA.crt -out CA.der.crt
```
Finally, you can install CA.der.crt on your device
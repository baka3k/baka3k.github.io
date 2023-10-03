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

After that, you can find `CA.crt` & `CA.pem` in your folder

*In additions, you can convert cert to DER format by command* 
```xml
openssl x509 -inform PEM -outform DER -in CA.crt -out CA.der.crt
```
Finally, you can install CA.der.crt on your device

# Gen CA EC KEY

Save following command to `genCA.sh`
```
openssl ecparam -genkey -name prime256v1 -noout -out private-key-ec.pem
openssl req -new -sha256 -key private-key-ec.pem -out certificate-signed-request-ec.csr
#Generate a self-signed certificate suitable for web servers:
openssl req -x509 -sha256 -days 365 -key private-key-ec.pem -in certificate-signed-request-ec.csr -out ec.cer
#Generate Keystore in the format of PKCS12:
openssl pkcs12 -export -name a -out ec.p12 -inkey private-key-ec.pem -in ec.cer
```
add executiton permission
```
chmod 777 genCA.sh
```
then execute command
```
./genCA.sh
```
after that, you can see `ec.p12, ec.cer, private-key-ec.pem,  certificate-signed-request-ec.csr` in your folder
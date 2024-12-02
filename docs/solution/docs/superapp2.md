# Secure Super App

Some time ago, I wrote a piece about Super Apps and posted it here  [https://medium.com/@baka3k/super-app-guideline-128abd24a711](https://medium.com/@baka3k/super-app-guideline-128abd24a711). I hope this will provide valuable insights for developers and businesses seeking to build a Super App as part of their digital transformation journey.

Let's summarize the key points:

# What's Super App?

From a **technical standpoint**, what constitutes a Super App?
<p align="center" width="100%">
<img align="center" width="77%" src="https://i.imgur.com/rtD7vJp.png">

</p>
An applications is called a Super App if include:

- Main App/Host App: Provides a runtime environment for Mini Apps
- Mini App: Can be delivered to users without going through App Store/PlayStore

> Note: Applications that do not meet the above criteria are **NOT** Super apps.

The development of a Super App requires careful consideration of numerous factors, such as:
- The app's **Fault Tolerance**
- The **Extensibility** of its architecture
- Its **Compatibility** with diverse frameworks: Android, IOS, React Js, React Native, Flutter...etc
- Its overall **Security**.
- ..etc

> This article provides a preliminary exploration of **several Security Concepts.**

## Super App Security Challenges
The Super App platform allows numerous MiniApps from various development teams to operate on it, with each team utilizing distinct methods to access resources via APIs provided by the Main App. Furthermore, the Super App facilitates integration with diverse platforms employing different programming languages and operating environments, which are unpredictable during the development phase. These factors render **Sensitive Data protection*** and **Fraud prevention** more challenging compared to conventional applications.

## Key security approaches for Super app

<p align="center" width="100%">
<img align="center" width="90%" src="https://i.imgur.com/ZjgVXYn.png">
</p>

## App Isolation
### Isolate Memory & State

Each MiniApp operates within its own isolated ReactNativeHost, ensuring a distinct runtime environment that prevents conflicts between MiniApps in terms of state, components, and third-party libraries.

Every ReactNativeHost is paired with a dedicated JavaScript Engine (e.g., Hermes, JCS), offering flexibility in selecting the most suitable engine for each MiniApp and mitigating engine-related conflicts.
This complete isolation, where each MiniApp has its own context, global variables, and memory, guarantees that changes within one MiniApp do not impact others.

By isolating MiniApps, the risk of one MiniApp's errors or vulnerabilities affecting others is minimized.

## Data Isolation
### Isolate Storage
MiniApps are granted exclusive permissions to specific directories at launch. Any access requests to directories ``outside`` of these designated boundaries are ``denied``

### Isolate Permission
MiniApps submit permission requests for system resources (Camera, Location, File) through a configuration file that is centrally managed and approved by a server-side CMS. 

**Inter-MiniApp permission delegation is prohibited**, and all data is governed by a uniform security policy.

*(I will provide a detailed explanation of the techniques involved in a separate article)*

## Security Communication
### SSL Pinning

<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/BoFrv0F.png">
</p>

- Generally, a smartphone application manages user information on a server so the information assets will move between the networks connecting them.

- The network-based malicious third party may access (sniff) any information during this communication or try to change information (data manipulation). 
- The malicious attacker in the middle (also referred to as "Man in The Middle") can also pretend to be the real server tricking the application.

> Apply SSL Pinning, refer: https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning

### Trust Mechanism

<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/g7nYrmv.png">
</p>

- MiniApps are `restricted from direct communication`. All interactions must be mediated by the MainApp, which acts as a security gateway to enforce access control policies.

### IPC - Inter-process communication
In a Super App ecosystem comprising Inhouse and Partner Applications, a well-defined IPC mechanism is essential to facilitate inter-application communication and data sharing, particularly when interacting with MiniApps.
- 1. Do not store decryption key in source code 
- 2. Do not store decryption key in application
- 3. Do not transfer Secret key for any reason
- 4. Use IPC to transfer public key and use it to encrypt data
- 5. Data must be protected at the same level ()

**Warning: Item 5 is critically important but often overlooked in the development process**

<p align="center" width="100%">
<img align="center" width="50%" src="https://i.imgur.com/IgWlGIp.jpg">
</p>
As you see:

- B has access to C. 
- A has ONLY access to B. 
However, A cannot access C directly, but can do so through B because B has not protected C's data at the same level.


### Generate encryption/decryption keys dynamically

<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/LzIyak9.png">
</p>

This architecture eliminates the need for statically stored keys. Generated key pairs are unique, and decryption capabilities are restricted to the applications that participated in the key generation process

### Share sensitive Data

<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/mG6yGLX.png">
</p>

- Step 2.1: Make sure App is protected by the OS's signature permission rule.
- Step 2.2: Make sure App is not re-signed - preventing attackers from injecting their key.
- Step 2.3: Signature is unique
    - Self Signature: Preventing the scenario of clearing the signature and re-signing
    - Calling Signature: verify calling package(same as step 4, verify calling package before return data)
- Step 4. Ensure all invocations are verified
### Generate KeyPair by Command
You can also try using existing Keypair by creating key pairs as follows

(Make sure you have installed [OpenSSL](https://www.openssl.org/))

```
openssl ecparam -genkey -name prime256v1 -noout -out private-key-ec.pem
openssl req -new -sha256 -key private-key-ec.pem -out certificate-signed-request-ec.csr
openssl req -x509 -sha256 -days 365 -key private-key-ec.pem -in certificate-signed-request-ec.csr -out ec.cer
openssl pkcs12 -export -name a -out ec.p12 -inkey private-key-ec.pem -in ec.cer
```

## Identify & Access
### Trust Manifest by Signature & Checksum MiniApp
<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/B6MIsUf.png">
</p>

### MiniApp WhiteList
Why do we need a MiniApp whitelist?
<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/ELlf9gN.png">
</p>
Based on the signature verification process, a MiniApp is trusted as long as the key pair is unchanged. This creates a potential attack vector where malicious actors could substitute legitimate bundles with compromised ones on the storage. Consequently, the Main App has no way to determine if the loaded MiniApp is free from vulnerabilities.
<p align="center" width="100%">
<img align="center" width="80%" src="https://i.imgur.com/BEPrIi4.png">
</p>

## Hardening
*(I will provide a detailed explanation of the techniques involved in a separate article)*
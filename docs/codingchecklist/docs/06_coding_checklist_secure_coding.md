---
layout: default
title: 06. Secure Coding
parent: 01. Android Coding checklist
nav_order: 62

---

### Saving Data
1. - [ ] [Mandatory] Use `EncryptedSharedPreferences` when store sensitive data
1. - [ ] [Mandatory] Do not hardcode key/password in source code, please use [local.properties or Secrets Gradle plugin](https://developers.google.com/maps/documentation/places/android-sdk/secrets-gradle-plugin)

### Sharing Data
 - [ ] [Mandatory] Sharing Data must be protected on a par with original data

### Connection
1. - [ ] [Mandatory] Do not use default `android:usesCleartextTraffic` && do not set `android:usesCleartextTraffic=true` to application tag in `AndroidManifest`

    __BAD__
    ```xml
    <application
            ...
            android:usesCleartextTraffic="true" // or do not define
            // below API 27, if not set, defaut value of usesCleartextTraffic is true
            // you must ensure that your application has the same behavior for all API level
            ...
    >
    </application>
    ```
    __GOOD__
    ```xml
    <application
            ...
            android:usesCleartextTraffic="false" 
            ....
    >
    </application>
    ```
1. - [ ] [Mandatory] in case of mandatory use Cleartext(http,fpt..etc) - use `network-security-config` for special domain

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <network-security-config>
        <domain-config cleartextTrafficPermitted="true">
            <domain includeSubdomains="true">your_domain.com</domain>
        </domain-config>
    </network-security-config>
    ```

1. - [ ] [Mandatory] Specify `'android:exported'`  for all android Components which defined in AndroidManifest

    ```xml
    <activity
        android:name="androidx.test.core.app.InstrumentationActivityInvoker$BootstrapActivity"
        android:exported="false" // do not use default
        android:theme="@android:style/Theme" >
    </activity>
    ```

    
# Project setup

```
$ npx react-native@latest init SslPinning
```

# SSL Pinning

## Public key hash

```
$ openssl rsa -in private.key -pubout -out public_key.pem
$ openssl rsa -in public_key.pem -pubin -outform DER | openssl dgst -sha256 -binary | openssl enc -base64
```

## Create network security config file (src/main/res/xml/network_security_config.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
 <domain-config cleartextTrafficPermitted="true">
    <domain includeSubdomains="true">10.0.2.2</domain>
 </domain-config>
 <base-config>
  <trust-anchors>
   <certificates src="@raw/__certificatename__<cert"/>
  </trust-anchors>
 </base-config>
 <pin-set>
  <pin digest="SHA-256">__publickeyhash__</pin>
 </pin-set>
</network-security-config>

```

## Copy the crt certificate file to src/main/res/raw

## Add config to AndroidManifest

```
<application
   ...
      android:networkSecurityConfig="@xml/network_security_config">
   ...
```

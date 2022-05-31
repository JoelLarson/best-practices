# GPG Management

Last Updated: `2022-05-03`

## Purpose

This document outlines setting up multiple GPG keys that meet the following criteria:

* Secure algorithm (preferring eliptic curve)
* Add multiple "primary" keys with their own Encrypt/Sign/Authenticate subkeys
* Backup commands for the keys and revoke certificates
* Anonymous and secure key refreshing

## Setup

```
# ~/.gnupg/gpg.conf
keyserver hkps://keys.openpgp.org

# hkps://keys.openpgp.org
keyserver hkp://zkaan2xfbuxia2wpf7ofnkbz6r5zdbbvxbunvp5g2iebopbfc4iqmbad.onion
```

## Create

The primary key is meant to be private. Derived keys (like signing key) are used for public consumption.

* Primary key (Certify)
* Signing key (Sign)
* Encrypt key (Encrypt)
* Authenticate key (Authenticate)

### Generate primary key

```
gpg --expert --full-generate-key

# Please select what kind of key you want:
#   (11) ECC

# Please select which elliptic curve you want:
#   (1) Curve 25519

# Please specify how long the key should be valid
#   (1y) 1 Year

# Fill in name and email address. Skip comment since it is useless atm.

# (O) for (O)kay

# Move mouse around screen, drag windows, etc to create entropy.
```

### Generate signing key
```
gpg --expert --edit-key <email-address>

> addkey

# Please select what kind of key you want:
#   (10) ECC (Sign only)

# Please specify how long the key should be valid.
#   (1m) 1 month

# Move mouse around screen, drag windows, etc to create entropy.
```


### Generate encrypt key
```
gpg --expert --edit-key <email-address>

> addkey

# Please select what kind of key you want:
#   (12) ECC (Encrypt only)

# Please specify how long the key should be valid.
#   (1m) 1 month

# Move mouse around screen, drag windows, etc to create entropy.
```


### Generate authenticate key
```
gpg --expert --edit-key <email-address>

> addkey

# Please select what kind of key you want:
#   (11) ECC (set your own capabilities)

# Possible actions for a ECDSA/EdDSA key: Sign Authenticate
#   Toggle the sign capability, Toggle the authenticate capability
#
# Current allowed actions: Authenticate

# Please specify how long the key should be valid.
#   (1m) 1 month

# Move mouse around screen, drag windows, etc to create entropy.
```

## Manage keys

### Delete a subkey
```
# Select the first subkey, will update to ssb* in output
key 1

# Delete the "selected" key(s)
delkey
```

## List keys

```
gpg --list-secret-keys

gpg --list-secret-keys --with-fingerprint
```

## Backup keys with revoke certificates

### Backup secret keys
```
gpg --export-secret-keys --armor <email-address>
```

### Create revoke certificates

Creating revoke certificates early allows them to be issued if the private key is lost.

```
gpg --gen-revoke --armor <email-address>
```

## Refresh keys

Refreshing your keys should happen on a regular basis. After setting a new expiration, relevant keys should be sent to your key server. 

```
gpg --edit-key <email-address>

# Select the first key shown starting with ssb
> key 1

# Expire the key to generate a new expiration date
> expire

> save
```

### Securely and anonymously synchronize with keyserver

(?todo I haven't been able to get this working yet due to tor)

```
paru -S parcimonie-sh-git
```

https://github.com/EtiennePerot/parcimonie.sh

## References

### Official
* https://www.gnupg.org/documentation/manpage.html
* https://keys.openpgp.org/about/usage
* https://wiki.archlinux.org/title/GnuPG

### Extra
* https://help.riseup.net/en/security/message-security/openpgp/best-practices
* https://spin.atomicobject.com/2013/11/24/secure-gpg-keys-guide/
* https://blog.tinned-software.net/create-gnupg-key-with-sub-keys-to-sign-encrypt-authenticate/

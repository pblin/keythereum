ethereumjs-keys
===============

[![Build Status](https://travis-ci.org/AugurProject/ethereumjs-keys.svg?branch=master)](https://travis-ci.org/AugurProject/ethereumjs-keys)
[![Coverage Status](https://coveralls.io/repos/AugurProject/ethereumjs-keys/badge.svg?branch=master&service=github)](https://coveralls.io/github/AugurProject/ethereumjs-keys?branch=master)

[![NPM](https://nodei.co/npm/ethereumjs-keys.png)](https://nodei.co/npm/ethereumjs-keys/)

Generate, import and export Ethereum private keys.  Uses PBKDF2 or scrypt key derivation functions.

Installation
------------

    $ npm install ethereumjs-keys

Usage
-----

```javascript
var ethKeys = require("ethereumjs-keys");

// User-specified password
var password = "wheethereum";

// Key derivation function (default: PBKDF2)
var kdf = "pbkdf2"; // "scrypt" to use the scrypt kdf

// Generate private key and the salt and initialization vector to encrypt it
var dk = ethKeys.create();
// dk:
{
    privateKey: <Buffer ...>,
    iv: <Buffer ...>,
    salt: <Buffer ...>
}

// Export key data to keystore "secret-storage" format:
// https://github.com/ethereum/wiki/wiki/Web3-Secret-Storage-Definition
var json = ethKeys.dump(password, dk.privateKey, dk.salt, dk.iv, kdf);
// json:
{
    address: '008aeeda4d805471df9b2a5b0f38a0c3bcba786b',
    Crypto: {
        cipher: 'aes-128-ctr',
        ciphertext: '5318b4d5bcd28de64ee5559e671353e16f075ecae9f99c7a79a38af5f869aa46',
        cipherparams: {
            iv: '6087dab2f9fdbbfaddc31a909735c1e6'
        },
        mac: '517ead924a9d0dc3124507e3393d175ce3ff7c1e96529c6c555ce9e51205e9b2',
        kdf: 'pbkdf2',
        kdfparams: {
            c: 262144,
            dklen: 32,
            prf: 'hmac-sha256',
            salt: 'ae3cd4e7013836a3df6bd7241b12db061dbe2c6785853cce422d148a624ce0bd'
        }
    },
    id: 'e13b209c-3b2f-4327-bab0-3bef2e51630d',
    version: 3
}

// In Node, export formatted JSON to file.
ethKeys.exportToFile(json);
```
After successful key export, you will see a message like:
```
Saved to file:
keystore/UTC--2015-08-11T06:13:53.359Z--008aeeda4d805471df9b2a5b0f38a0c3bcba786b

To use with geth, copy this file to your Ethereum keystore folder (usually ~/.ethereum/keystore).
```

Tests
-----

Unit tests are run with mocha.

    $ npm test

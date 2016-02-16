# BIPPath

Bitcoin BIP32 ("HD Wallet") path helpers.

There are multiple path representations being used by different implementations. These includes:
- `m/44'/0'/0'/0/0` where the apostrophe means hardened key
- `m/44h/0h/0h/0/0` where the letter `h` means hardened key
- and a binary representation predominantly used by Trezor and compatible wallets

Some useful links:
- [https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki](BIP32)
- [https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki](BIP44)
- [https://dcpos.github.io/bip39/](BIP39, an online tool to convert/derive keys)
- [https://bip32jp.github.io/english/index.html](BIP32JP, an online tool to convert/derive keys)


### API

- `BIPPath.fromString(path)` - creates an instance with a path written as text
- `BIPPath.fromTrezor(path)` - creates an instance with a path as an array (Trezor-style)
- `new BIPPath(path)` - alias for `BIPPath.fromTrezor(path)`
- `<bippath>.toTrezor()` - returns a Trezor-style array
- `<bippath>.toString(noRoot, oldStyle)` - returns a text encoded path. Set to `noRoot` to true to omit the `m/` prefix. Set `oldStyle` true to use `h` instead of `'` for marking hardened nodes.

Trezor-style arrays contain each node as a separate number, where hardened nodes are marked by setting the 32th bit: `m/44'/1/1/0` corresponds to `[ 44 | 0x80000000, 1, 1, 0 ]`


### Examples

```js
var bippath = require('bippath')

bippath.fromTrezor([44 | 0x80000000, 1, 1, 0]).toString() // m/44'/1/1/0

bippath.fromString("44'/0'/0'").toString(true) // m/44h/0h/0h

bippath.fromString("44h/0h/0'").toString() // m/44'/0'/0'

bippath.fromString("44'/0'/0'").toTrezor() // [ 0x80000044, 0x80000000, 0x80000000 ]
```
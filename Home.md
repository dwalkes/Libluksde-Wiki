libluksde is a library to access LUKS Disk Encryption encrypted volumes.

Project information:
* Status: experimental
* Licence: LGPLv3+

Supported encryption methods:
* AES (AES-CBC)

Unsupported encryption methods:
* AES (AES-ECB)
* AES (AES-XTS)
* anubis
* cast5
* cast6
* serpent
* twofish

Supported initialization vector modes:
* ESSIV (SHA1, SHA256)
* null
* plain, plain64

Unsupported initialization vector modes:
* benbi
* lmk
* plumb

Supported hashing methods:
* SHA1
* SHA224
* SHA256

Unsupported hashing methods:
* RIPEMD160

Work in progress:
* Thread-safety in volume API functions

Planned:
* Dokan library support
* Python 3 support

For more information see:
* [How to build from source](https://github.com/libyal/libluksde/wiki/Building)
* [Downloads](https://github.com/libyal/libluksde/releases)
* [Documentation](https://github.com/libyal/libluksde/tree/master/documentation)


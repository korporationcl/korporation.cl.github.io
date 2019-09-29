# SSL

## Why does SSL protocol exists?

Two main reasons are:

- Security: encrypt the data from the client to the server (in transit).
- Identification: Make sure you the computer you are talking to with is the one you trust.

## Encryption

without encrypting the packages in transit someone can intercept the data/sniff and read sensitive data (man in the middle)

### How encryption works (handshake)

1. Computers agree on how to encrypt

Client sends a `hello` package to the server. The information on the header inclues the `key`, `cipher` and `hash`.
The key is what Key the will use to exchange data. The ciper is how the data will be encrypted. The hash is used to check the integrity of the messages transmitted. Finally the version of SSL is sent (TLS 3.3) and the random number that is used to compute the master secret.

| Key | Cipher | Hash
| **RSA** | RC4 | **HMAC-MD5**
| Diffie-Helman | Triple DES | HMAC-SHA
| DSA | **AES** |


Server side:
| Key | Cipher | Hash
| **RSA** | RC4 | **HMAC-MD5**
| Diffie-Helman | Triple DES | HMAC-SHA
| DSA | **AES** |

| Version | 3.3
| Random Number | 12412414124214214

2. Server sends certificate
3. Your computer says 'starts encrypting'
4. The server says 'starts encrypting'
5. ALL messages are now encrypted


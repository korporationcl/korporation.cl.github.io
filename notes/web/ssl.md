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

Server sends certificate with information, such as:

- Who this server belongs to or `issuer`
- How long the certificate is valid for
- Public key


3. Your computer says 'starts encrypting'

- Client key exchange (both computers can calculate a master secret code)
- Client asks server to encrypt
- Client is finished and sends a message back to the server.

4. The server says 'starts encrypting'

- Server starts sending encrypted messages using the `cipher` agreed before.
- Last message sent back to the client goes encrypted already.

5. ALL messages are now encrypted

- Messages are encrypted at this stage

## Identitication (Trust)

1. Company asks a CA for a certificate (Verisign, SSLRapid, etc)

In order to ask for a certificate we need to send some information (CSR):
- Web server
- What company is this
- Where is the company located
- etc

2. CA creates/issues a certificate and signs it
3. Certificate is deployed in the Server/Load Balancer
4. Browser issued with root certificates
5. Browser trusts correctly signed certs


## References

- https://www.youtube.com/watch?v=iQsKdtjwtYI&t=388s



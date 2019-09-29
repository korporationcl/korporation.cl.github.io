# Server Name Indication (SNI)

Server Name Indication (SNI) allows the server to safely host multiple TLS Certificates for multiple sites, all under a single IP address. It adds the hostname of the server (website) in the TLS handshake as an extension in the CLIENT HELLO message. This way the server knows which website to present when using shared IPs.

- https://www.globalsign.com/en/blog/what-is-server-name-indication/
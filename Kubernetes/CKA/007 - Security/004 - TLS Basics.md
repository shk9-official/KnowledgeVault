
### TLS Basics

- Step 1 - User connects to server. Server responds back with server's public key
- Step 2 - User generates a symmetric key
- Step 3 - User encrypts symmetric key with server's public key, and then sends the encrypted key to server
- Step 4 - Server decrypts the encrypted key using server's public key. Server will now have decrypted symmetric key
- Step 5 - User and server can use the symmetric key to communicate securely, via a secure communication tunnel
- Certificate authorities (CAs) sign the certificates which they issue using their private keys
	- Users/clients can use the corresponding certificate authority's public key to validate the signature
- In case of SSH, the user creates a public+private key pair, and uploads the public key onto the server.
	- User then uses the private key to securely authenticate and connect to the server
- Client's can also get certificates by generating a certificate signing request (CSR), and getting a certificate issues by a CA
	- Sometimes servers request client for certificates as part of authentication validation


---

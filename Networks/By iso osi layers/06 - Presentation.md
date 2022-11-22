Presentation layer
---

Can be a way to include Encryption and Decryption to a protocol.
It can also note the received/sent string encoding to ease up with swapping out the encoding.

---

TLS & SSL
---

ssl is deprecated;

#### TLS hanshakes;
Are based on certificate authority+public/private key cryptography;
Initial connection is initialized with a TLS(ssl) handshake;

The handshake can be conducted either with RSA key exchange or with Diffie-Hellman exchange(sharing a publically known number with secrets on both clients and server which can be used to derive a common secret number);


#### TLS 1.2:

RSA:
1. Client sends in "hello", TLS version supported, ciphers supported and a random stream of bytes(client random). 
2. Server sends his "hello", which dictates how the reset of the "conversation" will go, it sends it's public certificate, cipher choosen and it's own random stream of bytes(server random).
3. The certificate contains information about which cert authority has signed their certificate, the certificates sign back into a "root" certificates which are stored and verified by the client.
Client sends an encrypted premaster secret, encrypted with the server's certificate.
4. Server decrypts the premaster secret.
Client, Server random and Premaster secret are used to derive the key Keys used in the current session get created.
Client sends a "finished" message that is encrypted with a session key.
Server sends a "finished"(? is it exactly "finished") message encrypted with a session key.
5. Secure symmetric encryption achieved;

The premaster secret can't be snooped on;
Yes the public key is the same for all clients;
But it is not feasiable to brute force all the possible premaster secrets possible with the public key to complete a MiTM/rogue router.
The client and server random are public.


Diffie-Hellman:
1. Client hello.
2. Server hello(ssl certificate, accepted ciphers), with an server signature, which contains (client random, server random, public) and it's (DH parameter, public). It encrypts this data(the server signature) with a private key(the server signature sent is public).
3. Client decrypts the digital signature with a public key received, those establishing that the server has the private key. Client then sends it's DH parameter to the server.
4. Client and server calculate the premaster secret; Client, Server random and Premaster secret are used to derive the key Keys used in the current session get created.
Client sends a "finished" message that is encrypted with a session key.
Server sends a "finished"(? is it exactly "finished") message encrypted with a session key.
5. Secure symmetric encryption achived.

#### TLS 1.3:

No RSA.
1. Client hello, TLS version, Public key
2. Server hello, certificate, Public key
3. Secret established with DH.
5. Secure symmetric encryption achived.

0-RTT resumption, PSK pre-shared keys(pre-established keys re-used; derived from session id and session ticket).


---

SSH tunnelling and port forwarding.
---



---

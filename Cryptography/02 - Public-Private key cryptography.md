Why do we need public, private key cryptography? 
------------------------------------------------
Public-private key cryptography depends on computational complexity.
A public key is nothing more than a derriative of a private key, stripped of information allowing for easy transformation into a private key or set of possible private keys.

Public keys can be shared freely with other people, while private keys cannot.

But what is a public key for?
--------------------------
Encrypting(anyone can encrypt a message as it's meant to be read only by the owner).
Verification(anyone can verify a message comes from the owner).

Likewise private keys do the opposite operations:
Decrypting(no one but the owner should be able to read a message).
Signing(no one but the owner should be able to proclaim identity).

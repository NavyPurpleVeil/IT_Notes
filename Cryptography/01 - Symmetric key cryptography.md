What does it mean that a key is symmetric?
------------------------------------------
A symmetric key can be used for both encryption and decryption.
This key can be derived from a password, using HMAC(Hash Message Authentication Code) an symmetric key can be verrified to be the true key.
(This way we avoid "destroying" the data on decryption with an invalid key).
Symmetrically encrypted data has little to no overhead.


What are encryption modes?
--------------------------
Encryption modes are different ways you can use the symmetric encryption.
In a way it's the different ways the key morphs(or doesn't) when being passed through encrypted data.
Various encryption modes give different levels of protection, speed etc.
For example an uncompressed image with a "plain"(same key for every block of data) encryption mode can leak metadata.

Methods such as counter(increment key by 1/X for each data block), or using encrypted data as a modificator to decrease leaked information.

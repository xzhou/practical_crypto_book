# Symmetric Key Encryption

Symmetric encryption uses a class of symmetric key ciphers to encrypt data. There are few symmetric encryption standard such as Blowfish and AES. For short, when in doubt which one to use, choose AES. Here are some difficulties when using them. 
a.	What mode to use? ECB, CBC, PCBC, OFB or CTR? If you are not sure which one to use, select CTR mode. In recent study, many implementations fail because developers failed to correctly generate nonce in CTR mode. I also recommend CBC with random IV. 
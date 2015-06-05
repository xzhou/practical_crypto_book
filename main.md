# Programmer's Handbook on Crypto


##Some general guide lines:

### It is not recommended to re-implement known protocol, for example, SSL or https. It is better to use existing libraries or schemes provided by the operating system. 


### 1. Key Derivation (how to generate keys from passwords?):

In many Samsung products, we need to generate encryption keys for file system encryption or sensitive data protection from user’s password. The best way to generate keys from password is using standard Password-Based Key Derivation Functions 2(PBKDF2) to generate keys. Depending on the usage scenario, we may also want to mix the password with some random salt before feeding them into the KDF functions. Android provided PBEKeySpec class for this kind of job. 
How many rounds for PBKDF2? This is depends on the device capability. More rounds, more secure. But we should not let the user wait for too long after password entering. 
Authentication:
Authentication is an activity to confirm the identity of an individual or an entity. In general, authentication is based on three factors: 
1.	What he has? (a OTP device, key card, et. al)
2.	What he is? (finger print, iris)
3.	What he knows. (password, private key)
Here are some techniques to do authentication: 
1.	User name and password. Most online system uses this method. However, it is important to protect the password. When login, it is recommended use HTTPs to encrypt the entire login message and all following messages exclusively using HTTPs or SSL. Do NOT partially encrypts a message because if not handled properly, it may subject to replay attack, MITM attacks. Like in our previous KLM design, we only partially encrypt the message and vulnerable to MITM attacks. (KVA257, KVA258).
2.	Certificate (use public key to verify the identity). This is mostly used to verify the server’s identity. For example, in https, the browser uses the certificate chain to verify the server’s identity. The biggest problem with certificate based authentication is programmer may not correctly verify the certificate chain. So it is recommended to use system provided trust manager to do the verification. See KVA346 for examples of failure. 
Typical attacks and defense:
Replay attack: Include timestamp or sequence number or nonce in the message. Then encrypt and sign or HMAC the message. 
MITM: The most important thing defense against MITM is verifying identity. Use HTTPs or SSL certificate to verify the identity of the server. Note that Diffie Hellman Key exchange protocol is vulnerable to MITM because it cannot verify the identity of the participating party. Use certificate to verify the server identity. 
Cases: If client and server already have a shared secret (or not), mutual authentication required (or not)
Confidentiality (Encryption):
Confidentiality prevents sensitive information from reaching the unauthorized party. The most common way to do this is encryption. 
1.	Asymmetric Encryption. 
Asymmetric encryption use mathematically linked key pairs to do “encryption”. Because it is slow compared to symmetric encryption, it is mostly used for key exchange.  Public key is used to verify and encrypt a message while private key is used to sign and decrypt message. Do NOT use private key to encrypt a message because it does not provide confidentiality since the public key is assumed to be publically known. 
2.	Symmetric encryption.
Symmetric encryption uses a class of symmetric key ciphers to encrypt data. There are few symmetric encryption standard such as DES, 3DES, Blowfish and AES. For short, when in doubt which one to use, choose AES. Here are some difficulties when using them. 
a.	What mode to use? ECB, CBC, PCBC, OFB or CTR? If you are not sure which one to use, select CTR mode. In recent study, many implementations fail because developers failed to correctly generate nonce in CTR mode. I also recommend CBC with random IV. 
Integrity (Detecting Unauthorized Data Modification): 
Integrity involves maintaining the consistency, accuracy and trustworthiness of a message. It is different from confidentiality. Encryption cannot guarantee integrity because the cipher text may be tempered. Typical techniques include:
1.	Signing: Use the private key to sign the message and verify the message with the public key. 
2.	HMAC: If a shared secret is available, we can use keyed hash to generate a message. 
In most cases, integrity and confidentiality are required at the same time. Please first encrypt then HMAC.
It is NOT recommended to design your own encryption scheme to achieve integrity and confidentiality. Use well-known authenticated encryption schemes such as Encrypt and MAC (E&M). 
Integrity & Confidentiality (Authenticated Encryption)
In most cases, we need Integrity and Confidentiality at the same time. To simplify the job, it is recommended using existing Authenticated Encryption schemes. 
More details?
https://cseweb.ucsd.edu/~mihir/cse207/s-ae.pdf
References:


Protection of Keys on the Host
Android/TIMA keystore, CCM. 
Access Control of Keys SEAndroid. 
Practical Implementation
Do not design protocols on your own. Use existing protocols.
Common questions:
1.	How the client & server trust each other? 
In most cases, server identity is guaranteed using certificate signed by trustable CA such as VeriSign and the client verifies it using public keys of the CA. Client is verified using username and password. 
2.	How to protect the data? 
On local machine, encryption is necessary to protect the data and we also need to transmit data security via SSL or HTTPs over the network.  
Future works on this document
1.	The way of generating signature 
2.	Anti-threat method for replay attack or others
3.	How to get security leverage with SE Android to store keys ...
4.	Some of them can have user credentials - ID and password which makes easy authentication but others cannot.

Appendix
Here is the original request (important parts in bold red): 
Hi Xiaoyong, Kunal,
You can find in the thread of emails below details of the discussion with the KNOX licensing management development team about the resolution of MITM vulnerabilities in the currently deployed ELM/KLM client-server protocols. Those vulnerabilities are being tracked under tickets KVA-257 and KVA-258. SIDI KVA is already handling the resolution of those tickets/vulnerabilities.
What we need your help is with the request for additional security advise from the licensing development team on the architecture and design for the evolution and future releases their components supporting what seems to be their roadmap. This request is summarized by the questions from 2 emails from Salva I reproduced below.
Please let me know if you have any questions.
Thanks, Jose
 
So, to spread out security enhancement to upcoming services  not only license system, I'd like to ask if we can get a sort of general security guideline (or manual) for client & server from SIDI.
I know each service has different protocols and interactions but this request begins to find out and share the best answers to the general questions:
    - How the client & server trusts each other? (authentication)
    - How to protect the  data? (confidentiality - securing data transsmission off of the device to the server)
 
I hope this guideline should be practical and can include the details like:
    - the way of generating signature 
    - anti threat method for replay attack or others
    - how to get security leverage with SE Android to store keys ...
With this guide line, I think engineering team can develop their services by considering the security more from the scratch.
 Our client can be one of the followings:
1. Downloadable or Prealoaded application running on Android/iOS
2. Preloaded KNOX related agents (services) or non-KNOX agents
    - Some of them can have user credentials - ID and password which makes easy authentication but others can not.
    - Some of them can use SE Android but others can not.
3. Web Client - Script application running on browser (e.g jQuery, AngularJS based script application)
 

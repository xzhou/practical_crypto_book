#Authentication

Authentication:
Authentication is an activity to confirm the identity of an individual or an entity. In general, authentication is based on three factors: 

1. What he has? (a OTP device, key card, et. al)
2. What he is? (finger print, iris)
3. What he knows. (password, private key)

Here are some techniques to do authentication: 
1.	User name and password. Most online system uses this method. However, it is important to protect the password. When login, it is recommended use HTTPs to encrypt the entire login message and all following messages exclusively using HTTPs or SSL. Do NOT partially encrypts a message because if not handled properly, it may subject to replay attack, MITM attacks. Like in our previous KLM design, we only partially encrypt the message and vulnerable to MITM attacks.

2.	Certificate (use public key to verify the identity). This is mostly used to verify the server’s identity. For example, in https, the browser uses the certificate chain to verify the server’s identity. The biggest problem with certificate based authentication is programmer may not correctly verify the certificate chain. So it is recommended to use system provided trust manager to do the verification. See KVA346 for examples of failure. 
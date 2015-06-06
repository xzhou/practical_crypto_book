# Key Derivation

In many situations, we need to generate encryption keys for file system encryption or sensitive data protection from user’s password. The best way to generate keys from password is using standard Password-Based Key Derivation Functions 2(PBKDF2) to generate keys. Depending on the usage scenario, we may also want to mix the password with some random salt before feeding them into the KDF functions. Android provided PBEKeySpec class for this kind of job. 
How many rounds for PBKDF2? This is depends on the device capability. More rounds, more secure. But we should not let the user wait for too long after password entering. 


# Examples
Java
```

```



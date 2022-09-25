## Key terms

* Ciphertext - The result of encrypting a plaintext, encrypted data

* Cipher - A method of encrypting or decrypting data. Modern ciphers are cryptographic, but there are many non cryptographic ciphers like Caesar.

* Plaintext - Data before encryption, often text but not always. Could be a photograph or other file

* Encryption - Transforming data into ciphertext, using a cipher.

* Encoding - NOT a form of encryption, just a form of data representation like base64. Immediately reversible.

* Key - Some information that is needed to correctly decrypt the ciphertext and obtain the plaintext.

* Passphrase - Separate to the key, a passphrase is similar to a password and used to protect a key, eg. SSH keys are protected with a passphrase

* Asymmetric encryption - Uses different keys to encrypt and decrypt.

* Symmetric encryption - Uses the same key to encrypt and decrypt.

* Brute force - Attacking cryptography by trying every different password or every different key

* Cryptanalysis - Attacking cryptography by finding a weakness in the underlying maths

* AES - stands for Advanced Encryption Standard. It was a replacement for DES which had short keys and other cryptographic flaws.


---
## How does Diffie Hellman Key Exchange work?
Alice and Bob want to talk securely. They want to establish a common key, so they can use symmetric cryptography, 
but they don’t want to use key exchange with asymmetric cryptography. 
This is where DH Key Exchange comes in.




---
## RSA & SSH

By default, SSH keys are RSA keys.
The ~/.ssh folder is the default place to store these keys for OpenSSH. 
The authorized_keys (note the US English spelling) file in this directory holds public keys that are allowed to access the server if key authentication is enabled. 
By default on many distros, key authentication is enabled as it is more secure than using a password to authenticate. 
Normally for the root user, only key authentication is enabled.

Specify a key for the standard Linux OpenSSH client:
```
ssh -i keyNameGoesHere user@host
```
### Backdoor
Leaving an SSH key in authorized_keys on a box can be a useful backdoor, 
and you don't need to deal with any of the issues of unstabilised reverse shells like Control-C or lack of tab completion.


---

## Tools to bypass RSA

* https://github.com/RsaCtfTool/RsaCtfTool
* https://github.com/ius/rsatool


### RSA in CTF

The key variables that you need to know about for RSA in CTFs are p, q, m, n, e, d, and c.

* “p” and “q” are large prime numbers, “n” is the product of p and q.

* The public key is n and e, the private key is n and d.

* “m” is used to represent the message (in plaintext) and “c” represents the ciphertext (encrypted text).

Example: p = 4391, q = 6659. n is 4391 x 6659 = 29239669

More about RSA -> https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/


---
## Symmetric vs asymmetric encryption

These are the 2 encryption categories:

* Symmetric encryption 
Uses the same key to encrypt and decrypt the data. 
Examples of Symmetric encryption are DES (Broken) and AES. 
These algorithms tend to be faster than asymmetric cryptography, and use smaller keys (128 or 256 bit keys are common for AES, DES keys are 56 bits long).


* Asymmetric encryption 
Uses a pair of keys, one to encrypt and the other in the pair to decrypt. Examples are RSA and Elliptic Curve Cryptography. 
Normally these keys are referred to as a public key and a private key. 
Data encrypted with the private key can be decrypted with the public key, and vice versa. 
Your private key needs to be kept private, hence the name. 
Asymmetric encryption tends to be slower and uses larger keys, for example RSA typically uses 2048 to 4096 bit keys.

Asymmetric encryption tends to be slower, so for things like HTTPS symmetric encryption is better.

The secret code represents a symmetric encryption key, 
The lock represents the server’s public key, 
The key represents the server’s private key.

Read more about encryption in HTTP -> https://robertheaton.com/2014/03/27/how-does-https-actually-work/

---
## Digital signatures

Digital signatures are a way to prove the authenticity of files, to prove who created or modified them. Using asymmetric cryptography, 
you produce a signature with your private key and it can be verified using your public key. 
The simplest form of digital signature would be encrypting the document with your private key, 
and then if someone wanted to verify this signature they would decrypt it with your public key and check if the files match.


Certificates are also a key use of public key cryptography, linked to digital signatures, commonly used is for HTTPS. 
How does your web browser know that the server you’re talking to is the real one?

The web server has a certificate that says it is the real one. 
The certificates have a chain of trust, starting with a root CA (certificate authority). 
Root CAs are automatically trusted by your device, OS, or browser from install. 
Certs below that are trusted because the Root CAs say they trust that organisation. 
Certificates below that are trusted because the organisation is trusted by the Root CA and so on. 
There are long chains of trust. 

---

## PGP & GPG
PGP stands for Pretty Good Privacy. It’s a software that implements encryption for encrypting files, performing digital signing and more.
GnuPG or GPG is an Open Source implementation of PGP from the GNU project. 
With PGP/GPG, private keys can be protected with passphrases in a similar way to SSH private keys. 
If the key is passphrase protected, you can attempt to crack this passphrase using John The Ripper and gpg2john. 
The key provided in this task is not protected with a passphrase.


---
## Encryption recomendations
The NSA recommends using RSA-3072 or better for asymmetric encryption and AES-256 or better for symmetric encryption. 
There are several competitions currently running for quantum safe cryptographic algorithms, 
and it’s likely that we will have a new encryption standard before quantum computers become a threat to RSA and AES.

Link -> https://nvlpubs.nist.gov/nistpubs/ir/2016/NIST.IR.8105.pdf


---

## QA

1. How do webservers prove their identity? - Certificates

2. What was the result of the attempt to make DES more secure so that it could be used for longer? - Triple DES




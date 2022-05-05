# Hash cracking tools

List of online tools for hash cracking:
* CyberChef -> https://gchq.github.io/CyberChef/
* Hashes -> https://hashes.com/en/decrypt/hash
* https://emn178.github.io/
* Cryptii -> https://cryptii.com/
* Bcrypt generator -> https://bcrypt-generator.com/
* Base64 Decode -> https://www.base64decode.org/
* Morse Code Translator -> https://morsecode.world/international/translator.html

---
## Identify a hash

* Hashes -> https://hashes.com/en/decrypt/hash
* Hashcat:
  ```
  hashid -m “<here_hash>”
  ```
  
---
## Cracking strong hashes faster

Some of encryption is very strong, so it can take a couple of hours or even days to break it.
If you have any clue:
* how long the password should be, 
* what is the min and max of characters, 
* if it contains only letters or numbers and other characters as well

You can use Hashcat more efficiently.

Example:

```
$ hashcat -m7100 hash.txt -a3 -1?l?u?d ?1?1?1?1?1?1?1?1 --increment --increment-min 6
```
* ?1?1?1?1?1?1?1?1 - number of characters (8)
* -1?l?u?d - type of characters
    Wider exclamention below:
    ```
    ?l = abcdefghijklmnopqrstuvwxyz
    ?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
    ?d = 0123456789
    ?h = 0123456789abcdef
    ?H = 0123456789ABCDEF
    ?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
    ?a = ?l?u?d?s
    ?b = 0x00 - 0xff
    ```
* --increment-min 6 - start with a mimimum length of 6 characters

Find more here: https://security.stackexchange.com/questions/201931/hashcat-specify-number-of-characters


--- 
## Cracking hashes with salt

* Hashcat:
  ```
  hashcat -m <hash_mode> <hash>:<salt> ../../wordlist.txt
  ```

---
## Hashcat

Most of hashes can be docoded via Hashcat.

### Example

Task: Decode hash in bcrypt $2*$, Blowfish (Unix).

1. Check hash-mode here -> https://hashcat.net/wiki/doku.php?id=example_hashes
   For bcrypt $2*$, Blowfish (Unix) it's `3200`.
2. Add hash to file:
   ```
   echo '$2a$05$LhayLxezLhK1LhWvKxCyLOj0j1u.Kj0jZ0pEmm134uzrQlFvQJLF6' > hash.txt
   ```
3. Decrypt hash:
   ```
   $ hashcat -m 3200 hash.txt
   ```
---
## Base32

Base32 can be decrypted via tools like https://emn178.github.io/ or in Linux command line
```
$ echo 'MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======' | base32 -d
```
---
## Base64

Base64 can be decrypted via tools like https://www.base64decode.org/ or in Linux command line
```
$ echo 'MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======' | base64 -d
```

---
## Rot13

Letters order has been changed

* https://rot13.com/

---
## Rot47

Letters order has been changed, but more obfuscated

* https://www.dcode.fr/rot-47-cipher

---


## Tips & tricks

### Super long hashes
If the length of hash it looks like the hash is encoded a multiple times use CyberChef.
It gaves you posibility to create a steps with different types of hashes, so you are able do successfully decrypt one hash by one.

* CyberChef -> https://gchq.github.io/CyberChef/






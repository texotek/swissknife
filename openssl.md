## 1. RSA

[RSA (Rivest–Shamir–Adleman)](https://en.wikipedia.org/wiki/RSA_(cryptosystem))

### 1.1 Creating the private key
To generate a private RSA key:
```bash
openssl genpkey -algorithm rsa -pkeyopt rsa_keygen_bits:8198 -out private_key.pem
```
- `-algorithm rsa`: Uses the RSA Algorithm
- `-pkeyopt rsa_keygen_bits:8198`: Number of bits in the generated key


### 1.2 Creating the public key
To create the public key, that you can share everywhere, from the generated private key, you can use:
```bash
openssl rsa -in private_key.pem -out public_key.pem -outform PEM -pubout
```

### 1.3 Encrypt a file using the public key

```bash
openssl pkeyutl -encrypt -inkey public_key.pem -pubin -in file.txt -out file.txt.rsa
```

### 1.4 Decrypt a file using the private key
```bash
openssl pkeyutl -decrypt -inkey private_key.pem -in file.txt.rsa -out file.txt
```

## 2. AES Symmetric Encryption

[Advanced Encryption Standard](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)

Encrypt the file using AES Password encryption:
```bash
openssl aes-256-cbc -pbkdf2 -iter 1000000 -e -a -in file.txt -out file.txt.aes # Here you will be prompted for a password
```

To decrypt the file after encrypting it:
```bash
openssl aes-256-cbc -pbkdf2 -iter 1000000 -d -a -in file.aes -out file.txt
```

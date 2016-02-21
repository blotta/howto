To encrypt a file named 'file1.txt' 

```
$openssl aes-256-cbc -a -salt -in file1.txt -out encrypted_file1.txt
```
 * aes-256-cbc : AES 256-bit encryption; [Electronic Codebook (ecb) | Cipher-block Chaining(cbc)(better)]

 * -a : store file in a readable format

 * -salt : safer password storage

To decrypt 'encrypted_file1.txt'
```
$openssl aes-256-cbc -d -a -in encrypted_file1.txt -out decrypted_file1.txt
```
 * -d: decrypt

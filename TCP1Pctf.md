## EncryptDecryptFile
This challenge used Mercurial which is a new thing i came across

Attachment given is a .hg file. 
A .hg file is related to Mercurial, a distributed version control system similar to Git. The .hg directory is created in the root of a repository to store version control information, 
including configurations, history, and metadata about the repository.
```
store/: Contains the actual data of the repository (commits, files, etc.).
dirstate: Tracks the state of the working directory (which files are modified, added, etc.).
branch: Stores information about the current branch.
hgrc: The configuration file for the repository.
requires: Lists the features required to read the repository (e.g., file formats, compression methods).
```

We have also been given a main.py which is an encryption-decryyption program fpr AES-256-CBC in whcih the key and iv are given

https://www.mercurial-scm.org/doc/hg.1.html
This page is a manpage for hg. From this, since we need to find the file that has been deleted, we had to use ```hg status```
whose output gave ```!flag.enc```

Since the '!' sign is there, it indicates the file is missing hence deleted. The changes can then be reverted using ``hg revert --all``

Now that we get the encrypted file, we can use the encryption key, iv and encryption method from mai.py to decrypt the file
```
OpenSSL is an open-source command line tool that is commonly used to generate private keys, create CSRs, install your SSL/TLS certificate, and identify certificate information
```
Command for decryption:

![image](https://github.com/user-attachments/assets/f2c31add-9f7c-4cbd-b643-0055d66c4616)

However, looking at the file signature, we can see that its supposed to be a png
changing the file extension and opening the file, we get the flag

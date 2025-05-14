🔐 GPG Encryption & Decryption Guide (Ubuntu + WSL Friendly)
============================================================

This guide walks through installing GPG, generating keys, sharing and importing public keys, encrypting a file for a specific recipient, and decrypting it on the receiver's end.

* * * * *

🧱 Step 1: Install GPG
----------------------

Install GPG on Ubuntu or WSL:

```
sudo apt update
sudo apt install gnupg -y
```

* * * * *

🔑 Step 2: Generate Your GPG Key Pair
-------------------------------------

To generate a key pair (public + private):

```
gpg --full-generate-key
```

You'll be prompted for:

-   **Key type**: Press Enter to choose the default (RSA and RSA).

-   **Key size**: Type `4096` for strong encryption.

-   **Key expiration**: Type `0` (never expires), or a number like `1y`, `6m`, etc.

-   **User ID info**:

    -   Name: e.g., Alice Example

    -   Email: e.g., alice@example.com

-   **Passphrase**: Enter a strong passphrase for your private key.

* * * * *

📄 Step 3: Export Your Public Key (To Share With Others)
--------------------------------------------------------

Export your public key to share it with someone (like Bob):

```
gpg --armor --export alice@example.com > alice-public.asc
```

This creates an ASCII file `alice-public.asc` which you can email or share.

* * * * *

🛅 Step 4: Import Someone Else's Public Key (e.g., Bob's)
---------------------------------------------------------

When someone shares their public key (`bob-public.asc`) with you:

```
gpg --import bob-public.asc
```

List imported keys:

```
gpg --list-keys
```

You'll see something like:

```
pub   rsa4096 2025-05-14 [SC]
      ABCD1234EFGH5678...
uid           [ultimate] Bob Smith <bob@example.com>
```

* * * * *

🔒 Step 5: Encrypt a File for a Specific Recipient
--------------------------------------------------

To encrypt `secret.txt` so only Bob can decrypt it:

```
gpg --encrypt --armor --recipient bob@example.com secret.txt
```

This creates `secret.txt.asc` --- an ASCII-armored encrypted file.

📅 You can also encrypt multiple files like this:

```
tar -czf files.tar.gz file1.txt file2.txt
gpg --encrypt --armor --recipient bob@example.com files.tar.gz
```

* * * * *

📤 Step 6: Share the Encrypted File
-----------------------------------

Now send `secret.txt.asc` (or `files.tar.gz.asc`) to Bob via email or a secure channel.

* * * * *

🔓 Step 7: Receiver Decrypts the File (Bob's Side)
--------------------------------------------------

Bob needs to:

### Install GPG (if not already):

```
sudo apt update && sudo apt install gnupg -y
```

### Import Alice's public key (optional, for verifying signatures):

```
gpg --import alice-public.asc
```

### Decrypt the file:

```
gpg --decrypt secret.txt.asc
```

Or, to save output:

```
gpg --output secret.txt --decrypt secret.txt.asc
```

Bob will be prompted for his passphrase (set when generating his key).

* * * * *

🔐 Bonus: Generate and Export Private Key for Backup
----------------------------------------------------

```
# Export your private key (store it securely!)
gpg --armor --export-secret-keys alice@example.com > alice-private.asc
```

🚨 Never share this file with anyone. It's your private key.

* * * * *

🧪 Bonus: Trusting an Imported Public Key (Optional but Recommended)
--------------------------------------------------------------------

If you trust the identity of the person whose key you imported:

```
gpg --edit-key bob@example.com
```

Then inside the GPG shell:

```
gpg> trust
```

Choose a trust level (5 for ultimate if you're sure it's really them), then:

```
gpg> quit
```

* * * * *

✅ Summary
---------

 🛠️ Action            💻 Command                                                               
---------
 Install GPG          `sudo apt install gnupg`                                                 

 Generate key         `gpg --full-generate-key`                                                

 Export public key    `gpg --armor --export you@example.com > public.asc`                     

 Import public key    `gpg --import someone.asc`                                               

 Encrypt file         `gpg --encrypt --armor --recipient someone@example.com file`            

 Decrypt file         `gpg --output file --decrypt file.asc`                                  

* * * * *

🩼 Tips
-------

-   Always verify the fingerprint of a public key if possible.

-   Do **not** share your private key.

-   Use `--sign` if you want to add a digital signature while encrypting.

* * * * *

Happy encrypting! 🔐

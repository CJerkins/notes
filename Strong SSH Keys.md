Your SSH keys might use one of the following algorithms:

    🚨 DSA: It’s unsafe and even no longer supported since OpenSSH version 7, you need to upgrade it!
    ⚠️ RSA: It depends on key size. If it has 3072 or 4096-bit length, then you’re good. Less than that, you probably want to upgrade it. The 1024-bit length is even considered unsafe.
    👀 ECDSA: It depends on how well your machine can generate a random number that will be used to create a signature. There’s also a trustworthiness concern on the NIST curves that being used by ECDSA.
    ✅ Ed25519: It’s the most recommended public-key algorithm available today!

`ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"`
`ssh-keygen -o -a 256 -t ed25519 -C "$(hostname)-$(date +'%d-%m-%Y')"`

You’ll be asked to enter a passphrase for this key, use the strong one. You can also use the same passphrase like any of your old SSH keys.

    -o : Save the private-key using the new OpenSSH format rather than the PEM format. Actually, this option is implied when you specify the key type as ed25519.
    -a: It’s the numbers of KDF (Key Derivation Function) rounds. Higher numbers result in slower passphrase verification, increasing the resistance to brute-force password cracking should the private-key be stolen.
    -t: Specifies the type of key to create, in our case the Ed25519.
    -f: Specify the filename of the generated key file. If you want it to be discovered automatically by the SSH agent, it must be stored in the default `.ssh` directory within your home directory.
    -C: An option to specify a comment. It’s purely informational and can be anything. But it’s usually filled with <login>@<hostname> who generated the key.

Adding Your Key to SSH Agent
----------------------------

You can find your newly generated private key at ~/.ssh/id_ed25519 and your public key at ~/.ssh/id_ed25519.pub. Always remember that your public key is the one that you copy to the target host for authentication.

Before adding your new private key to the SSH agent, make sure that the SSH agent is running by executing the following command:

eval "$(ssh-agent -s)"

Then run the following command to add your newly generated Ed25519 key to SSH agent:

ssh-add ~/.ssh/id_ed25519

Or if you want to add all of the available keys under the default .ssh directory, simply run:

ssh-add

ref:
https://cryptsus.com/blog/how-to-secure-your-ssh-server-with-public-key-elliptic-curve-ed25519-crypto.html
https://medium.com/risan/upgrade-your-ssh-key-to-ed25519-c6e8d60d3c54
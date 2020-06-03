# SSH

Each computer really only needs to ever generate one (1) SSH public / private key pair. Typically lives in the `~/.ssh` folder called `id_rsa` (private) and `id_rsa.pub` (public).

Then when you want to tell a server that it is okay for your computer to access it, you just copy the public key over to the server. e.g. to ssh into clear, we need to copy our public key to clear.

A server might have many SSH keys of many different agents that are allowed to access it. For example my github has 3 ssh keys. One from MBP, one from my GoDaddy server and one from my Clear server. This allows all three of those computers to access my github account.

https://help.dreamhost.com/hc/en-us/articles/216499537-How-to-configure-passwordless-login-in-Mac-OS-X-and-Linux



https://www.tecmint.com/ssh-passwordless-login-using-ssh-keygen-in-5-easy-steps/



#### Example: ssh into clear

Generate the key:

```bash
[local]$ ssh-keygen -t rsa
```

This creates a public/private keypair of the type (-t) rsa.

Copy the key from local to Clear (appending to `authorized_keys` file):

```bash
[local]$ cat ~/.ssh/key.pub | ssh psd2@ssh.clear.rice.edu "cat >> ~/.ssh/authorized_keys"
```

Add custom key to ssh-client (only necessary if key has a custom name not `id_rsa`):

```bash
[local]$ eval `ssh-agent` # this clears previously added keys
[local]$ ssh-add ~/.ssh/key # add the key
[local]$ ssh-add -l # list keys
```

You can then ssh in:

```bash
[local]$ ssh psd2@ssh.clear.rice.edu
```

Alternatively you can specify the key to use when you ssh in:

```bash
[local]$ ssh -i ~/.ssh/key psd2@ssh.clear.rice.edu
```


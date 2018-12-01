# SSH

Each computer really only needs to ever generate one (1) SSH public / private key pair. Typically lives in the `~/.ssh` folder called `id_rsa` (private) and `id_rsa.pub` (public).

Then when you want to tell a server that it is okay for your computer to access it, you just copy the public key over to the server. A server might have many SSH keys of many different agents that are allowed to access it. For example my github has 3 ssh keys. One from MBP, one from my GoDaddy server and one from my Clear server. This allows all three of those computers to access my github account.
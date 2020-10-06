# K4YT3X's Hardened OpenSSH Server Configuration

This repository hosts my hardened version of OpenSSH server configuration file.

**Please review the configuration file carefully before applying it.** You are responsible for actions done to your own system.

## Usages

1. Download the file `sshd_config` from the repository
1. **Review the content of the `sshd_config` file to make sure all settings are suitable for your system**
1. Backup your current `/etc/ssh/sshd_config` file
1. Overwrite the old `sshd_config` file with the downloaded `sshd_config` file
1. Run the appropriate command to restart the SSH service (e.g., `sudo systemctl restart ssh`)

```shell
# clone the repository
git clone https://github.com/k4yt3x/sshd_config.git ~/sshd_config

# backup the original sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# replace the old sshd_config with the new one
sudo cp ~/sshd_config/sshd_config /etc/ssh/sshd_config

# apply changes
sudo systemctl restart ssh

# remove the downloaded repository if you don't need it anymore
rm -rf ~/sshd_config
```

For convenience, I have pointed the URL `https://akas.io/sshd` to the `sshd_config` file. You may therefore download the `sshd_config` file with the following command. However, be sure to check the integrity of the file after downloading it if you choose to download using this method.

```shell
curl -sSL akas.io/sshd -o sshd_config
```

You may want to use the [ssh-audit](https://github.com/arthepsy/ssh-audit) script to check the cryptographic strength of your SSH server after done configuring it.

## `sshd_config` Content

```properties
# Name: K4YT3X Hardened OpenSSH Configuration
# Author: K4YT3X
# Date Created: October 5, 2020
# Last Updated: October 5, 2020
# Version: 1.0

# SSH server listening address and port
#Port 22
#ListenAddress 0.0.0.0
#ListenAddress ::

# only listen to IPv4
#AddressFamily inet

# only listen to IPv6
#AddressFamily inet6

# accept locale-related environment variables
AcceptEnv LANG LC_*

# disallow ssh-agent forwarding to prevent lateral movement
AllowAgentForwarding no

# prevent TCP ports from being forwarded over SSH tunnels
AllowTcpForwarding no

# permit only the specified users to login
#AllowUsers k4yt3x

# challenge-response authentication backend it not configured by default
# therefore, it is set to "no" by default to avoid the use of an unconfigured backend
ChallengeResponseAuthentication no

# explicitly define cryptography algorithms to avoid the use of weak algorithms
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
HostKeyAlgorithms rsa-sha2-512,rsa-sha2-256,ssh-rsa,ssh-ed25519
KexAlgorithms curve25519-sha256@libssh.org,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,diffie-hellman-group14-sha256
MACs umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com

# number of client alive messages sent without client responding
ClientAliveCountMax 2

# set session timeout to 300 seconds
# disconnects the user after being idle for 5 minutes
ClientAliveInterval 300

# compression before encryption might cause security issues
Compression no

# disallow remote hosts from connecting to forwarded ports
# i.e. forwarded ports are forced to bind to 127.0.0.1 instad of 0.0.0.0
GatewayPorts no

# prevent SSH trust relationships from allowing lateral movements
IgnoreRhosts yes

# log verbosely for addtional information
#LogLevel VERBOSE

# set maximum authenticaion retries to prevent brute force attacks
MaxAuthTries 3

# allow a maximum of two multiplexed sessions over a single TCP connection
MaxSessions 2

# disable password authentication
#PasswordAuthentication no

# disallow connecting using empty passwords
PermitEmptyPasswords no

# prevent root from being logged in via SSH
PermitRootLogin no

# suppress MOTD
PrintMotd no

# enforce SSH server to only use SSH protocol version 2
# SSHv1 contains security issues and should be avoided at all costs
# SSHv1 is disabled by default after OpenSSH 7.0, but this option is
#   specified anyways to ensure this configuration file's compatibility
#   with older versions of OpenSSH server
Protocol 2

# enable pubkey authentication
PubkeyAuthentication yes

# override default of no subsystems
Subsystem sftp /usr/libexec/openssh/sftp-server

# let ClientAliveInterval handle keepalive
TCPKeepAlive no

# disable reverse DNS lookups
UseDNS no

# disable PAM since it is not necessary
# disables PasswordAuthentication
#UsePAM no
UsePAM yes

# disable X11 forwarding since it is not necessary
X11Forwarding no
```

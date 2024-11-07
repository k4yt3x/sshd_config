# K4YT3X's Hardened OpenSSH Server Configuration

This repository hosts my hardened version of OpenSSH server (>=8.5) configuration file. You will need to modify the configuration according to the error messages if you want to use this configuration for older versions of OpenSSH.

**Please review the configuration file carefully before applying it.** You are responsible for actions done to your own systems.

In addition to this configuration, you may also want to check out the [SSH Hardening Guides](https://www.ssh-audit.com/hardening_guides.html).

## Usages

1. Download the file `sshd_config` from the repository
1. **Review the content of the `sshd_config` file to make sure all settings are suitable for your system**
1. Backup your current `/etc/ssh/sshd_config` file
1. Overwrite the old `sshd_config` file with the downloaded `sshd_config` file
1. Run the appropriate command to restart the SSH service (e.g., `sudo systemctl restart ssh`)

```shell
# download the configuration file from GitHub using curl or other methods
curl https://raw.githubusercontent.com/k4yt3x/sshd_config/master/sshd_config -o ~/sshd_config

# backup the original sshd_config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# replace the old sshd_config with the new one
sudo mv ~/sshd_config /etc/ssh/sshd_config

# make sure the file has the correct ownership and permissions
sudo chown root:root /etc/ssh/sshd_config
sudo chmod 644 /etc/ssh/sshd_config

# use systemctl to reload the SSH server and apply the new configurations
# on some distributions the SSH server service is called sshd
sudo systemctl restart ssh
```

For convenience, I have pointed the URL `https://kt.ax/sshd` to the `sshd_config` file. You may therefore download the `sshd_config` file with the following command. However, be sure to check the integrity of the file after downloading it if you choose to download using this method.

```shell
curl -L kt.ax/sshd -o sshd_config
```

It's recommended to use the [ssh-audit](https://github.com/jtesta/ssh-audit) script to check the cryptographic strength of your SSH server after done configuring it.

## Deactivating Short Diffie-Hellman Moduli

Diffie-Hellman moduli used for `diffie-hellman-group-exchange-sha256` should be at lest 3072 bits long according to [Mozilla's OpenSSH server hardening guide](https://infosec.mozilla.org/guidelines/openssh#modern-openssh-67). This can be done with the following commands.

```shell
# backup original moduli file
cp /etc/ssh/moduli /etc/ssh/moduli.backup

# find lines with moduli >= 3071 bits and save them to moduli.tmp
awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.tmp

# overwrite original moduli file with the updated one
mv /etc/ssh/moduli.tmp /etc/ssh/moduli
```

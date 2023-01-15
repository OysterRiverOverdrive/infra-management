## Set Up For Remote Configuration

If you need to attach to a wifi and don't have a UI yet.

```
nmcli device wifi list
sudo nmcli device wifi connect SSID_or_BSSID password password
```

Install and start the ssh server to accept connections.

```
sudo pacman -S openssh

sudo systemctl enable sshd
sudo systemctl start sshd
```

Add the public ssh key for the authorized client.

```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
curl -Lo ~/.ssh/authorized_keys https://raw.githubusercontent.com/OysterRiverOverdrive/infra-management/main/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

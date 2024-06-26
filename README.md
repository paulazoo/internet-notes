# Useful Links:
- https://dnsleaktest.com
- https://www.speedtest.net/

# Software links:
- http://squidman.net/squidman/
    - IP Binding
- www.airvpn.org
    - VPN w __port forwarding__, split tunneling, kill switch, so works in China
    - Bind to clients and test disconnection for leaks
- https://gpgtools.org/
    - GNU Privacy Guard
- https://librewolf.net/
    - Librewolf with uBlock Origin ![](./uBlock_origin_settings.png)
- https://developers.cloudflare.com/1.1.1.1/setup/
    - Cloudflare DNS resolver instead of ISP's DNS resolver ![](./cloudflare_dns.png) ![](./DNS_servers.png)
    ```
    1.1.1.1
    1.0.0.1
    2606:4700:4700::1111
    2606:4700:4700::1001
    ```
    - to check DNS resolvers: `scutil --dns | grep 'nameserver\[[0-9]*\]'`
    - remember to delete all DNS servers for joining public wifis though
- https://www.torproject.org/download/

# Setting up a VPS:
- Mac Terminal syntax: ssh user@publicIP
    - e.g. `ssh root@167.172.252.178`
- Windows: PuTTY
- TEXT MELISSA PASSWORDS TO STOP FORGETTING THEM
- Install java from terminal: `apt install openjdk-19-jre-headless`
- For file stuff
    - Winows: WinSCP
    - Mac: Finder sucks. Cyberduck https://cyberduck.io/download/
    - use SFTP protocol, port 22

# Remote Desktop (xubunutu-desktop) for VPS
- install desktop (xubuntu is faster than ubuntu): `sudo apt install xubuntu-desktop`
- install xrdp: `sudo apt install xrdp --disable-ipv6`
- check xrdp has auto-started: `sudo systemctl status xrdp`
    - to disable (inter-dependent with xrdp-sesman): `sudo systemctl disable xrdp && sudo systemctl disable xrdp xrdp-sesman`
- add a user for remote desktop: `sudo adduser [user name]`
    - list users: `less /etc/passwd`
    - change password: `sudo passwd [user name]`
- Allow RDP port 3389: `sudo ufw allow 3389/tcp`
    - firewall customization: `sudo nano /etc/default/ufw`
    - `sudo ufw disable` and `sudo ufw enable`
- restart xrdp: `service xrdp restart`
- `sudo reboot` takes some time...

__Troublshooting__:
- Stop xrdp `sudo service xrdp stop`
- Replace these lines from the xrdp start script: `sudo nano /etc/xrdp/startwm.sh`
```
test -x /etc/X11/Xsession && exec /etc/X11/Xsession 
exec /bin/sh /etc/X11/Xsession
```
with
```
startxfce4
```
- Restart xrdp with `sudo service xrdp start`


- permission denied errors: `sudo chmod -R 777 <directory_name>`


# Common Ports:
- 20 & 21: FTP
- 22: SSH
- 23: Telnet
- 3389: RDP
- 80: HTTP
- 443: HTTPS
- 53: DNS
- 25: SMTP
- 110: POP3
- 143: IMAP


# GnuPG
- gpg --full-generate-key
- gpg --output ~/revocation.crt --gen-revoke reachpaulazhu@gmail.com
- gpg --list-keys --keyid-format=long
- gpg --list-secret-keys --keyid-format=long
```
sec => 'SECret key'
ssb => 'Secret SuBkey'
pub => 'PUBlic key'
sub => 'public SUBkey'
```
- gpg --output public.pgp --armor --export reachpaulazhu@gmail.com
- gpg --output example.gpg --encrypt --recipient reachpaulazhu@gmail.com example.txt
- gpg --output example --decrypt example.gpg
- gpg --import [public_key_file]
- gpg --verify [signature_file.sig] [file_to_verify]
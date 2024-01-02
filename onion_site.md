# Requirements:
- Ubuntu 16.04.1 (both virtual box and remote work)
- Tor [https://www.torproject.org/docs/debian.html.en](https://www.torproject.org/docs/debian.html.en) (Ubuntu Xenial Xerus, Tor, Stable)

# Steps:
### 1) Build mitmproxy 0.17
```sh
sudo -i # become root

apt-get install aptitude # optional

aptitude install libffi-dev libjpeg-dev libssl-dev libxml2-dev libxslt1-dev libyaml-dev python-dev python-pip zlib1g-dev

pip install --upgrade pip

pip install mitmproxy

mitmproxy --version
```

### 2) Configure onion site

Edit `/etc/tor/torrc` to include something like the following in the location-hidden services section:

```sh
HiddenServiceDir /var/lib/tor/bbc-onion/
HiddenServicePort 80  localhost:20080
```
### 3) Restart Tor

Run `sh /etc/init.d/tor restart`

### 4) Get onion site name
Run `sh cd /var/lib/tor/bbc-onion cat hostname`

### 5) Enable mitproxy
Make `run-proxy.sh`:
```
#!/bin/sh

<insert-given-hostname>.onion

SITE=<insert-hosted-site-url>.com

# escape dots in regexps
Dotify() {
    echo $1 | sed -e 's/\./\\./g'
}

# we are building a 1:1 mapping for only the 'www' site, not the whole TLD
SITE=www.$SITE
ONION=www.$ONION

# sub-edit the regular expressions
SITE_RE=`Dotify $SITE`
ONION_RE=`Dotify $ONION`

# tell the user
echo "when you press return, onion access for $SITE will become available on:"
echo http://$ONION
echo "...press return to launch the proxy. Use 'q' to exit the proxy."
read junk

# run
exec mitmproxy \
    -p 20080 \
    -R http://${SITE} \
    --anticache \
    --replace ":~hq .:${ONION_RE}:${SITE}" \
    --replace ":~hs .:${SITE_RE}:${ONION}" \
    --replace ":~bs . ~t \"application/json\":${SITE_RE}:${ONION}" \
    --replace ":~bs . ~t \"application/x-javascript\":${SITE_RE}:${ONION}" \
    --replace ":~bs . ~t \"text/css\":${SITE_RE}:${ONION}" \
    --replace ":~bs . ~t \"text/html\":${SITE_RE}:${ONION}" \
    --replace ":~s:${SITE_RE}:${ONION}"
```

* the `~hq` directive edits the *H*eaders in the re*Q*uest 
* the `~hs` directive edits the *H*eaders in the re*S*ponse
* the `~bs` directive edits the *B*ody of the re*S*ponse
  * ...the `~t` directive limits this rewriting to responses of certain content *T*ype (eg: `text/html`)
* the `~s` directive edits the whole of the (any kind of) re*S*ponse

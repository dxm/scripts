# acme

As root:

```
useradd -m acme
dnf install acme-tiny-core
mkdir -p /var/lib/acme
chown acme:acme /var/lib/acme
semanage fcontext -a -t cert_t "/var/lib/acme(/.*)?"
restorecon -r -v /var/lib/acme
mkdir /var/www/challenge
chown acme:acme /var/www/challenge
semanage fcontext -a -t httpd_sys_content_t "/var/www/challenge(/.*)?"
restorecon -r -v /var/www/challenge
echo "acme ALL = NOPASSWD: /usr/bin/systemctl restart httpd" > /etc/sudoers.d/acme
```

As acme:

```
cd /var/lib/acme
# change example.com to domain
openssl req -new -sha256 -key server.key -subj "/" -reqexts SAN -config <(cat /etc/ssl/openssl.cnf <(printf "[SAN]\nsubjectAltName=DNS:example.com,DNS:www.example.com")) > server.csr
mkdir ~/bin
cp renew ~/bin
ACME_VAR=/var/lib/acme
ACME_WWW=/var/www/challenge
ACME_BIN=/usr/sbin/acme_tiny
KEY=$ACME_VAR/account.key
CSR=$ACME_VAR/server.csr
CRT=$ACME_VAR/server.crt
LOG=$HOME/renew.log
$ACME_BIN --account-key $KEY --csr $CSR --acme-dir $ACME_WWW > $CRT
```

# DNS Config for your Cube

Based on the [Yunohost doc](https://yunohost.org/#/dns_config).

## use the TLD (domaine.tld)

```
@ 1800 IN A 111.222.333.444 # (Minimal) IPv4
@ 1800 IN AAAA 2001:AABB:CCDD:EEFF:1122:3344:5566:7788 # IPv6
```

Redirection from the domain name and subdomains to the IP address

```
* 1800 IN A 111.222.333.444 # Wildcard: *.domain.tld and domain.tld redirection to the IP address
* 1800 IN AAAA 2001:AABB:CCDD:EEFF:1122:3344:5566:7788
```


XMPP

```
_xmpp-client._tcp 1800 IN SRV 0 5 5222 domain.tld. # (Minimal) clients connection
_xmpp-server._tcp 1800 IN SRV 0 5 5269 domain.tld. # (Minimal) servers connection

muc 1800 IN CNAME @ # multi-user chat rooms at muc.domain.tld
anonymous 1800 IN CNAME @ # connection without account at `anonymous.domain.tld`
bosh 1800 CNAME @ # BOSH
_xmppconnect 1800 TXT "_xmpp-client-xbosh=https://bosh.domain.tld:5281/http-bind"
pubsub 1800 IN CNAME @
vjud 1800 IN CNAME @
```

Email

```
@ 1800 IN MX 10 domain.tld. # (Minimal)
@ 1800 IN TXT "v=spf1 a mx -all"
```

## use a subdomain (cube.domaine.tld)

```
cube 1800 IN A 111.222.333.444 # (Minimal) IPv4
cube 1800 IN AAAA 2001:AABB:CCDD:EEFF:1122:3344:5566:7788 # IPv6
```

Redirection from the domain name and subdomains to the IP address

```
*.cube 1800 IN A 111.222.333.444 # Wildcard: *.cube.domain.tld and cube.domain.tld redirection to the IP address
*.cube 1800 IN AAAA 2001:AABB:CCDD:EEFF:1122:3344:5566:7788
```

XMPP

```
_xmpp-client._tcp 1800 IN SRV 0 5 5222 cube.domain.tld. # (Minimal) clients connection
_xmpp-server._tcp 1800 IN SRV 0 5 5269 cube.domain.tld. # (Minimal) servers connection

muc.cube 1800 IN CNAME cube # multi-user chat rooms at muc.domain.tld
anonymous.cube 1800 IN CNAME cube # connection without account at `anonymous.domain.tld`
bosh.cube 1800 CNAME cube # BOSH
_xmppconnect 1800 TXT "_xmpp-client-xbosh=https://bosh.cube.domain.tld:5281/http-bind"
pubsub.cube 1800 IN CNAME cube
vjud.cube 1800 IN CNAME cube
```

Email

```
cube 1800 IN MX 10 cube.domaine.tld. # (Minimal)
cube 1800 IN TXT "v=spf1 a mx -all"
```
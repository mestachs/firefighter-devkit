# Networking

## DNS what ?

### Unknown entry

```shell {line-numbers}
curl -vvv http://neverexisteddomains.com
* Rebuilt URL to: http://neverexisteddomains.com/
* Could not resolve host: neverexisteddomains.com
* Closing connection 0
curl: (6) Could not resolve host: neverexisteddomains.com
```


```shell
dig neverexisteddomains.com

; <<>> DiG 9.10.3-P4-Ubuntu <<>> neverexisteddomains.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 43906
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;neverexisteddomains.com.	IN	A

;; Query time: 1 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Mon Jul 08 21:04:20 CEST 2019
;; MSG SIZE  rcvd: 41
```


```shell
whois neverexisteddomains.com
No match for "NEVEREXISTEDDOMAINS.COM".
>>> Last update of whois database: 2019-07-08T19:04:39Z <<<
```

### Known entry

```shell
dig google.be

; <<>> DiG 9.10.3-P4-Ubuntu <<>> google.be
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 1690
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;google.be.			IN	A

;; ANSWER SECTION:
google.be.		255	IN	A	172.217.168.227

;; Query time: 31 msec
;; SERVER: 127.0.1.1#53(127.0.1.1)
;; WHEN: Mon Jul 08 21:05:14 CEST 2019
;; MSG SIZE  rcvd: 54
```

whois might find some info

```sh
whois google.com
   Domain Name: GOOGLE.COM
   Registry Domain ID: 2138514_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.markmonitor.com
   Registrar URL: http://www.markmonitor.com
   Updated Date: 2018-02-21T18:36:40Z
   Creation Date: 1997-09-15T04:00:00Z
   Registry Expiry Date: 2020-09-14T04:00:00Z
   Registrar: MarkMonitor Inc.
   Registrar IANA ID: 292
   Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
   Registrar Abuse Contact Phone: +1.2083895740
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
   Name Server: NS1.GOOGLE.COM
   Name Server: NS2.GOOGLE.COM
   Name Server: NS3.GOOGLE.COM
   Name Server: NS4.GOOGLE.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2019-07-09T14:48:20Z <<<
```

but note that whois has now less value since GDPR is here.

### Classic issues

#### Ends with .

#### Propagation delay

#### Aggressive cache

eg java will cache nearly forever, except if you tell him so.

https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html

## HTTPS

### Self signed certificate

```sh
curl -vvv https://self-signed.badssl.com/
*   Trying 104.154.89.105...
* Connected to self-signed.badssl.com (104.154.89.105) port 443 (#0)
* found 148 certificates in /etc/ssl/certs/ca-certificates.crt
* found 597 certificates in /etc/ssl/certs
* ALPN, offering http/1.1
* SSL connection using TLS1.2 / ECDHE_RSA_AES_128_GCM_SHA256
* server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
* Closing connection 0
curl: (60) server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none
More details here: http://curl.haxx.se/docs/sslcerts.html

curl performs SSL certificate verification by default, using a "bundle"
 of Certificate Authority (CA) public keys (CA certs). If the default
 bundle file isn't adequate, you can specify an alternate file
 using the --cacert option.
If this HTTPS server uses a certificate signed by a CA represented in
 the bundle, the certificate verification probably failed due to a
 problem with the certificate (it might be expired, or the name might
 not match the domain name in the URL).
If you'd like to turn off curl's verification of the certificate, use
 the -k (or --insecure) option.
```

```sh
curl -vvv -k https://self-signed.badssl.com/
*   Trying 104.154.89.105...
* Connected to self-signed.badssl.com (104.154.89.105) port 443 (#0)
* found 148 certificates in /etc/ssl/certs/ca-certificates.crt
* found 597 certificates in /etc/ssl/certs
* ALPN, offering http/1.1
* SSL connection using TLS1.2 / ECDHE_RSA_AES_128_GCM_SHA256
* 	 server certificate verification SKIPPED
* 	 server certificate status verification SKIPPED
* 	 common name: *.badssl.com (matched)
* 	 server certificate expiration date OK
* 	 server certificate activation date OK
* 	 certificate public key: RSA
* 	 certificate version: #3
* 	 subject: C=US,ST=California,L=San Francisco,O=BadSSL,CN=*.badssl.com
* 	 start date: Wed, 12 Jun 2019 15:31:59 GMT
* 	 expire date: Fri, 11 Jun 2021 15:31:59 GMT
* 	 issuer: C=US,ST=California,L=San Francisco,O=BadSSL,CN=*.badssl.com
* 	 compression: NULL
* ALPN, server accepted to use http/1.1
> GET / HTTP/1.1
> Host: self-signed.badssl.com
> User-Agent: curl/7.47.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.10.3 (Ubuntu)
< Date: Mon, 08 Jul 2019 19:03:07 GMT
< Content-Type: text/html
< Content-Length: 477
< Last-Modified: Fri, 21 Jun 2019 23:18:34 GMT
< Connection: keep-alive
< ETag: "5d0d65ca-1dd"
< Cache-Control: no-store
< Accept-Ranges: bytes
< 
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="shortcut icon" href="/icons/favicon-red.ico"/>
  <link rel="apple-touch-icon" href="/icons/icon-red.png"/>
  <title>self-signed.badssl.com</title>
  <link rel="stylesheet" href="/style.css">
  <style>body { background: red; }</style>
</head>
<body>
<div id="content">
  <h1 style="font-size: 12vw;">
    self-signed.<br>badssl.com
  </h1>
</div>

</body>
</html>
* Connection #0 to host self-signed.badssl.com left intact
```

### External assessment

https://www.ssllabs.com/ssltest/

### Test your client library

https://badssl.com/


## ssh

### Permission denied

```sh
ssh bad@github.com
Permission denied (publickey).
```

Which certificate is used to authenticate ?

```sh
ssh -vvv bad@github.com
OpenSSH_6.6.1, OpenSSL 1.0.1k-fips 8 Jan 2015
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 56: Applying options for *
debug2: ssh_connect: needpriv 0
debug1: Connecting to github.com [140.82.118.3] port 22.
debug1: Connection established.
debug1: identity file /home/ec2-user/.ssh/id_rsa type -1
debug1: identity file /home/ec2-user/.ssh/id_rsa-cert type -1
debug1: identity file /home/ec2-user/.ssh/id_dsa type -1
debug1: identity file /home/ec2-user/.ssh/id_dsa-cert type -1
debug1: identity file /home/ec2-user/.ssh/id_ecdsa type -1
debug1: identity file /home/ec2-user/.ssh/id_ecdsa-cert type -1
debug1: identity file /home/ec2-user/.ssh/id_ed25519 type -1
debug1: identity file /home/ec2-user/.ssh/id_ed25519-cert type -1
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_6.6.1
debug1: Remote protocol version 2.0, remote software version babeld-93408c70
debug1: no match: babeld-93408c70
debug2: fd 3 setting O_NONBLOCK
debug3: load_hostkeys: loading entries for host "github.com" from file "/home/ec2-user/.ssh/known_hosts"
debug3: load_hostkeys: found key type RSA in file /home/ec2-user/.ssh/known_hosts:1
debug3: load_hostkeys: loaded 1 keys
debug3: order_hostkeyalgs: prefer hostkeyalgs: ssh-rsa-cert-v01@openssh.com,ssh-rsa-cert-v00@openssh.com,ssh-rsa
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug2: kex_parse_kexinit: curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1
debug2: kex_parse_kexinit: ssh-rsa-cert-v01@openssh.com,ssh-rsa-cert-v00@openssh.com,ssh-rsa,ecdsa-sha2-nistp256-cert-v01@openssh.com,ecdsa-sha2-nistp384-cert-v01@openssh.com,ecdsa-sha2-nistp521-cert-v01@openssh.com,ssh-ed25519-cert-v01@openssh.com,ssh-dss-cert-v01@openssh.com,ssh-dss-cert-v00@openssh.com,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,ssh-ed25519,ssh-dss
debug2: kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
debug2: kex_parse_kexinit: aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se
debug2: kex_parse_kexinit: hmac-md5-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-ripemd160-etm@openssh.com,hmac-sha1-96-etm@openssh.com,hmac-md5-96-etm@openssh.com,hmac-md5,hmac-sha1,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
debug2: kex_parse_kexinit: hmac-md5-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-ripemd160-etm@openssh.com,hmac-sha1-96-etm@openssh.com,hmac-md5-96-etm@openssh.com,hmac-md5,hmac-sha1,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96
debug2: kex_parse_kexinit: none,zlib@openssh.com,zlib
debug2: kex_parse_kexinit: none,zlib@openssh.com,zlib
debug2: kex_parse_kexinit: 
debug2: kex_parse_kexinit: 
debug2: kex_parse_kexinit: first_kex_follows 0 
debug2: kex_parse_kexinit: reserved 0 
debug2: kex_parse_kexinit: ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256
debug2: kex_parse_kexinit: ssh-dss,rsa-sha2-512,rsa-sha2-256,ssh-rsa
debug2: kex_parse_kexinit: chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr,aes256-cbc,aes192-cbc,aes128-cbc
debug2: kex_parse_kexinit: chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr,aes256-cbc,aes192-cbc,aes128-cbc
debug2: kex_parse_kexinit: hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: kex_parse_kexinit: hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-sha1-etm@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-sha1
debug2: kex_parse_kexinit: none,zlib,zlib@openssh.com
debug2: kex_parse_kexinit: none,zlib,zlib@openssh.com
debug2: kex_parse_kexinit: 
debug2: kex_parse_kexinit: 
debug2: kex_parse_kexinit: first_kex_follows 0 
debug2: kex_parse_kexinit: reserved 0 
debug2: mac_setup: setup hmac-sha1-etm@openssh.com
debug1: kex: server->client aes128-ctr hmac-sha1-etm@openssh.com none
debug2: mac_setup: setup hmac-sha1-etm@openssh.com
debug1: kex: client->server aes128-ctr hmac-sha1-etm@openssh.com none
debug1: kex: ecdh-sha2-nistp256 need=20 dh_need=20
debug1: kex: ecdh-sha2-nistp256 need=20 dh_need=20
debug1: sending SSH2_MSG_KEX_ECDH_INIT
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: Server host key: RSA 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48
debug3: load_hostkeys: loading entries for host "github.com" from file "/home/ec2-user/.ssh/known_hosts"
debug3: load_hostkeys: found key type RSA in file /home/ec2-user/.ssh/known_hosts:1
debug3: load_hostkeys: loaded 1 keys
debug3: load_hostkeys: loading entries for host "140.82.118.3" from file "/home/ec2-user/.ssh/known_hosts"
debug3: load_hostkeys: loaded 0 keys
debug1: Host 'github.com' is known and matches the RSA host key.
debug1: Found key in /home/ec2-user/.ssh/known_hosts:1
Warning: Permanently added the RSA host key for IP address '140.82.118.3' to the list of known hosts.
debug1: ssh_rsa_verify: signature correct
debug2: kex_derive_keys
debug2: set_newkeys: mode 1
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug2: set_newkeys: mode 0
debug1: SSH2_MSG_NEWKEYS received
debug1: SSH2_MSG_SERVICE_REQUEST sent
debug2: service_accept: ssh-userauth
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug2: key: /home/ec2-user/.ssh/id_rsa ((nil)),
debug2: key: /home/ec2-user/.ssh/id_dsa ((nil)),
debug2: key: /home/ec2-user/.ssh/id_ecdsa ((nil)),
debug2: key: /home/ec2-user/.ssh/id_ed25519 ((nil)),
debug1: Authentications that can continue: publickey
debug3: start over, passed a different list publickey
debug3: preferred gssapi-keyex,gssapi-with-mic,publickey,keyboard-interactive,password
debug3: authmethod_lookup publickey
debug3: remaining preferred: keyboard-interactive,password
debug3: authmethod_is_enabled publickey
debug1: Next authentication method: publickey
debug1: Trying private key: /home/ec2-user/.ssh/id_rsa
debug3: no such identity: /home/ec2-user/.ssh/id_rsa: No such file or directory
debug1: Trying private key: /home/ec2-user/.ssh/id_dsa
debug3: no such identity: /home/ec2-user/.ssh/id_dsa: No such file or directory
debug1: Trying private key: /home/ec2-user/.ssh/id_ecdsa
debug3: no such identity: /home/ec2-user/.ssh/id_ecdsa: No such file or directory
debug1: Trying private key: /home/ec2-user/.ssh/id_ed25519
debug3: no such identity: /home/ec2-user/.ssh/id_ed25519: No such file or directory
debug2: we did not send a packet, disable method
debug1: No more authentication methods to try.
Permission denied (publickey).
```

```
debug2: key: /home/ec2-user/.ssh/id_rsa ((nil)),
debug2: key: /home/ec2-user/.ssh/id_dsa ((nil)),
debug2: key: /home/ec2-user/.ssh/id_ecdsa ((nil)),
debug2: key: /home/ec2-user/.ssh/id_ed25519 
```

Note that the look up policy can be influenced by `~/.ssh/config`
In the worstcase scenario, you have plenty of available keys and ssh try them all and then server bans you for too much trials. :(

## My app can't connect to the db

Ideally try as if in the network of your app.

Possible causes :

* firewall ? vlan ?
* running on non-default port ?
* correct url (host, port,...) ? 
* correct credentials ?
* storage available ? lack of disk space can lead to strange behavior
* server is really down ?

Tools to diagnose if the error message isn't that clear

### try with a command line tool

if the command line tool is available : psql, mysql,... or mssql-cli

```
mssql-cli -P xxxxx -U admin -S dbinstancename.environment.eu-central-1.rds.amazonaws.com
Error message: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: TCP Provider, error: 35 - An internal exception was caught)
```

### try with telnet
```
telnet dbinstancename.environment.eu-central-1.rds.amazonaws.com 1433
Trying 18.123.456.12...
Connected to ec2-18-123-456-12.eu-central-1.compute.amazonaws.com.
Escape character is '^]'.

^C^ZConnection closed by foreign host.
```

### try with netcat

telnet is often not installed by default, may be you have netcat

```
nc -v dbinstancename.environment.eu-central-1.rds.amazonaws.com 1443
nc: getaddrinfo: Name or service not known

```



## nmap


### self checking


```
nmap -sV github.com

Starting Nmap 7.01 ( https://nmap.org ) at 2019-07-08 21:26 CEST
Nmap scan report for github.com (140.82.118.4)
Host is up (0.039s latency).
rDNS record for 140.82.118.4: lb-140-82-118-4-ams.github.com
Not shown: 996 filtered ports
PORT     STATE SERVICE   VERSION
22/tcp   open  ssh       (protocol 2.0)
80/tcp   open  http
443/tcp  open  ssl/https GitHub.com
9418/tcp open  git?
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
```

You probably don't want to expose your db, redis, mongo,... server directly from the internet
nmap help

### Making the web secure, one unit test at a time

http://gauntlt.org/

https://github.com/gauntlt/gauntlt/blob/master/examples/nmap/simple.attack



```
@slow
Feature: simple nmap attack (sanity check)

  Background:
    Given "nmap" is installed
    And the following profile:
      | name     | value      |
      | hostname | scanme.nmap.org |

  Scenario: Verify server is available on standard web ports
    When I launch an "nmap" attack with:
      """
      nmap -p 80,443 <hostname>
      """
    Then the output should match /80.tcp\s+open/
    And the output should not match:
      """
      443/tcp\s+open
      """

```


## wireshark


Sipsniff is a very simple sniffer for SIP protocol that allows us to filter by SIP method type.

# Features #
Sipsniff allows us to:
  * Capture SIP packets in show then in plaintext
  * Filter INVITE messages
  * Filter authentication digest

# Usage #
```
$ perl sipsniff.pl 

SipSNIFF v1.2 - by Pepelux <pepeluxx@gmail.com>
-------------

Usage: sudo perl -i <interface> sipsniff.pl [options]
 
== Options ==
-i  <string>     = Interface (ex: eth0)
-p  <integer>    = Port (default: 5060)
-m  <string>     = Filter method (ex: INVITE, REGISTER)
-u               = Filter authentication digest
```

  * Caturing SIP packets:
```
$sudo perl sipsniff.pl -i eth0
```
  * Filtering authentication digest:
```
$sudo perl sipsniff.pl -i eth0 -m INVITE
```

# Examples #
```
$ sudo perl sipsniff.pl -i eth0 -u
Digest username="100", realm="asterisk", nonce="1d6ff7c4", uri="sip:192.168.0.55", response="e41413259e3cfe923559104d6b3a1a66", algorithm=MD5
Digest username="101", realm="asterisk", nonce="009ae95a", uri="sip:192.168.0.55", response="737c1434dbb718d6573b739ea1d2fcb2", algorithm=MD5
Digest username="1000",realm="asterisk",nonce="155bbc9a",uri="sip:9011972595821839@192.168.0.55",response="aa8ed081d55fd7f7ba823f4737333f2c",algorithm=MD5
```

Mmmm yes, the last capture (user=1000) is a real attack while I was testing the script in my honeypot :-O

```
$ sudo perl sipsniff.pl -i eth0
[+] 192.168.0.18:5070 => 192.168.0.55:5060
INVITE sip:100@192.168.0.55 SIP/2.0
Via: SIP/2.0/UDP 192.168.0.55:5070;branch=vswvnucpgrx8hldhn7by2u60h1bspxj9yqkmlu1u729ypfvty2f2lmas4ukphg6clsm139f
From: "100" <sip:100@192.168.0.55>;tag=0c26cd11
To: <sip:100@192.168.0.55>
Contact: <sip:100@192.168.0.55:5070;transport=udp>
Call-ID: 8bb857dabec36b80a43eb1ae4ce1927b
CSeq: 1 INVITE
User-Agent: sipptk
Max-Forwards: 70
Allow: INVITE,ACK,CANCEL,BYE,NOTIFY,REFER,OPTIONS,INFO,SUBSCRIBE,UPDATE,PRACK,MESSAGE
Content-Type: application/sdp
Content-Length: 183

v=0
o=anonymous 1312841870 1312841870 IN IP4 192.168.0.55
s=session
c=IN IP4 192.168.0.55
t=0 0
m=audio 2362 RTP/AVP 0
a=rtpmap:18 G729/8000
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000


[+] 192.168.0.55:5060 => 192.168.0.18:5070
SIP/2.0 401 Unauthorized
Via: SIP/2.0/UDP 192.168.0.55:5070;branch=vswvnucpgrx8hldhn7by2u60h1bspxj9yqkmlu1u729ypfvty2f2lmas4ukphg6clsm139f;received=84.127.4.109;rport=5070
From: "100" <sip:100@192.168.0.55>;tag=0c26cd11
To: <sip:100@192.168.0.55>;tag=as1ac87c24
Call-ID: 8bb857dabec36b80a43eb1ae4ce1927b
CSeq: 1 INVITE
Server: Asterisk PBX 1.8.13.1~dfsg1-3+deb7u3
Allow: INVITE, ACK, CANCEL, OPTIONS, BYE, REFER, SUBSCRIBE, NOTIFY, INFO, PUBLISH
Supported: replaces, timer
WWW-Authenticate: Digest algorithm=MD5, realm="asterisk", nonce="5e54bf74"
Content-Length: 0
```
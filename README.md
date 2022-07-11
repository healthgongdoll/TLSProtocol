# TLSProtocol

## What is TLS Protocol

Transport Layer Security (TLS) protocol is known as the security layer that located beyond Transport Layer (TCP/UDP) and under Application Layer (HTTP/SMTP/Other Application Protocol)

## Why we use TLS protocol

- Using unprotected data from an application can be used in crimes and harms innocent users 
- Add Protection on top of TCP (Authentication & Integrity) 
- Designed to operate over TCP (TLS itself does not deal with timing and packet retransmission) 

## How come we use TLS protocol 

Original version of TLS first developed as SSL (Secure Socket Layer) publicly published in 1995 under the help of Netscape (Version 2).
- Due to the vulnerabilities (DROWN), SSL versions were kept updated (Until version 3.0)

In 2006, IETF introduced new extension version of SSL but under the name of TLS 1.0 (To avoid legal issues with Netscape) 
- TLS 1.0: First version of modern day security protocol 
- TLS 1.1 ~ TLS 1.2: Mostly used protocol (99.7% in 2019) 
- TLS 1.3: Latest version of the TLS protocol 

The reason why TLS 1.2 is still major compare to TLS 1.3 is that similar issue with IPV4 changing IPV6. It is hard to change everyone at one time 

## Where we use TLS Protocol

The Application of TLS is HTTPS (HTTP over TLS)
- It encrypts HTTP protocol to make it more secure 

When we use plain HTTP protocol 
```
GET /hello.txt HTTP/1.1
User-Agent: curl/7.63.0 libcurl/7.63.0 OpenSSL/1.1.l zlib/1.2.11
Host: www.example.com
Accept-Language: en
HTTP/1.1 200 OK
Date: Wed, 30 Jan 2019 12:14:39 GMT
Server: Apache
Last-Modified: Mon, 28 Jan 2019 11:17:01 GMT
Accept-Ranges: bytes
Content-Length: 12
Vary: Accept-Encoding
Content-Type: text/plain
```

```
GET /hello.txt HTTP/1.1
User-Agent: curl/7.63.0 libcurl/7.63.0 OpenSSL/1.1.l zlib/1.2.11
Host: www.example.com
Accept-Language: en
t8Fw6T8UV81pQfyhDkhebbz7+oiwldr1j2gHBB3L3RFTRsQCpaSnSBZ78Vme+DpDVJPvZdZUZHpzbbcqmSW1+3xXGsERHg9YDmpYk0VVDiRvw1H5miNieJeJ/FNUjgH0BmVRWII6+T4MnDwmCMZUI/orxP3HGwYCSIvyzS3MpmmSe4iaWKCOHQ==
```

## Format of TLS

TLS organized with two sub layers (Record & Handshake)

### HandShake Layer
- HandShake
- Change Cipher Spec
- Alert

### Record Layer 
- Fragmentation
- Compression
- Authentication
- Encryption

## TLS Protocol Session 

Association between client and server 
- Two parties have common information when session established (Session Parameters) 
- Necessary to make establishment of a session but not sufficient (Need Connection) 

### Session Parameters 
- SessionID 
- Peer Certificate
- Compression Method
- Cipher Suite 
- Master Secret 
- Session Ticket 

## TLS Protocol Connection

One session can consisted of many connection 
- No need to establish session again 
- Session can be suspended and resume again 
- To resume an old session and create a new connection (Skip negotiation and make shorter (No master secret creation required)) 

Session Roles are different, Connection Roles are same 
- In a session one must be client and server
- In a connection both parties are peers 

To start communication they need to exchange **Two Random Numbers**, **Create/Use Master Secret**, **Read/Write keys and Parameters** (Connection state parameters) 



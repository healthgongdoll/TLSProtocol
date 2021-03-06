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
- In Total, they have 6 different secrets (3 Read/ 3 Write Secret)

## TLS Protocol Connection 6 Secrets 

1. Server and Client Random Numbers (Exchanged) - Sequence of bytes chosen by server and client for connection
2. Sever / Client MAC secret - MAC key for message integrity. (Each for sign/verify the message vice versa) 
3. Server / Client write secret - encryption key for server and client

## TLS Protocol Session Resumption 

### Normal TLS Protocol (2 RTT)

![image](https://user-images.githubusercontent.com/79100627/178326160-df607ace-a5e8-458d-a950-e867e8242350.png)

### TLS Protocol with Ticket (1 RTT)

![image](https://user-images.githubusercontent.com/79100627/178326241-a24ec336-63f1-4e87-ac5c-c2ba374f9ca0.png)

## TLS SubLayers 

### HandShake Layer (Establish Session / Initialize Communication)

1. Handshake Protocol - provide security parameters for Record Protocol (Establish Session)
2. Change Cipher Spec Protocol - signals readiness of cryptographic features (Establish Connection) 
3. Alter Protocol - report abnormal condition

### Record Layer (Data Transfer)

1. Recrod Protocol - carries message from protocols in HandShake 


## TLS Handshake Protocol 
- Assist Client & Server agree on TLS version 
- Authenticate client & server 
- Negoitate MAC Algorithm
- Negotiate Encryption Algorithm and Key 

4 Main Phases 

![image](https://user-images.githubusercontent.com/79100627/178331355-fff01443-8b3d-492f-bc1f-ea2a04d615dc.png)

## Phase I 

### ClientHello 

I. Agree on TLS version, Algorithm for key exchange, message authentication and encryption, compression method, session ID and 2 random numbers (for master secret) 
- Version - highest TLS version that client support 
- Random - 32-bit timestamp & 28 bytes generated by a secure random number generator - to serve as nonce to prevent replay attack and used during pre-master key exchange 
- Session ID - variable length session identifier - a non zero value (wish to update parameter of existing connection) or create a new connection on this session. A zero value indicate that client wishes to establish a new connection on a new session 
- CipherSuite - list that contains cryptographic algorithms supported by the client in decreasing order of preference 
- Compression Method - list of compression method 

### ServerHello 
- version - lower of two version numbers 
- random - same procedure of client hello
- session id - if session id sent by client is not zero, server will search for previously cached session and if a match is found that session id will be use; other wise a new session will created server return 0 
- cipher suite - single cipher suite selected by server from those proposed by client - if supported, server will agree on client's preferred cipher suite 
- compression method - compression method selected by server from those proposed by client 

### Cipher Suite 

![image](https://user-images.githubusercontent.com/79100627/178332678-c7d751cc-86c5-4747-a593-a893483aa8af.png)

Downgrade attack possible 

## Phase II 

Server Authentication and Key Exchange (X.509 Certificates) 

- A chan of certificates: Root, Intermediate, and Server 
- Server Key Exchange: After Certificate message, server sends a ServerKeyExchange message (Pre- Master Secret) 
- Certificate Request: Sent if server needs to authenticate clinet itself 
- ServerHello done: Signals the client that Phase II is over 

### Difference between Pre-Master Secret vs Master Secret vs Read/Write Key 

Pre-master Secret: value exchanged and generated by client and server (only client and server know pre-master secret)
Master Secret: generated by both using premaster secret and nonce 
Session Key: 4 or 6 sequence of 32 bytes generated using master secret & nonces and key block algorithm: 2 Mac Keys, 2 Encryption Keys 

![image](https://user-images.githubusercontent.com/79100627/178635747-d4abc338-4212-4599-92b3-1772f4384e8f.png)

## Phase III

Client Key Exchange and Authentication: Client sends up to 3 messages back to the server - but only after it has verified that server provided a valid certificate and acceptable Hello Server Parameters 

- Certificate: Chain of certificates
- ClientKeyExchange: Client Public Key 
- Certificate Verify: Hash code to prove certificate 

## Phase IV 

Finalizing and Finishing

- Change CipherSpec = message that is actually part of ChangeCipherSpec Protocol: Show that client/server has moved all of the cipher suite and parameter form (Encryption Begins) 
- Finished = Encrypted and Authenticated message that announces the end of handshake protocol by client/server

## TLS Change Cipher Spec Protocol 

Simple Protocol (1 byte) makes previously negotiated parameters effective so communication become encrypted 

## TLS Alert Protocol 

Used to convey TLS - Related Alerts (warnings and errors) to peer entity (2bytes only) 

## TLS Record Layer (Protocol)
 
 - Responsible for Confidentiality and Integrity of data 
 - fragmenting higher layer protocol data in to blocks (2^14 bytes or less)
 - Optionally Compressing Data
 - Add Message Authentication Code 
 - Encryption 
 - Add TLS record header 

![image](https://user-images.githubusercontent.com/79100627/178327970-34984ba9-32ed-4437-92e8-9ea7cbb4e354.png)

![image](https://user-images.githubusercontent.com/79100627/178328089-584dbd64-5a45-4e44-9e71-b61b001f1511.png)

## TLS Record Protocol Record Header 

Record Header = Packet of Record Protocol Consist of header (TLS header) & payload (TLS payload) 
- Transfer Application data
- Transfer Messages in ChangeCipherSpec
- Transfer Alert Protocol 

## TLS Header
![image](https://user-images.githubusercontent.com/79100627/178328794-8a1cca20-1545-40bb-ad11-437fce7b2183.png)

### Content Type 
```
0x14 - ChangeCipherSpec
0x15 - Alert 
0x16 - Handshake 
0x17 - Application
```
### Version
2 Bytes long identify Major and Minor version of TLS 

### Length 
2 Bytes long field - identify the length of paylaod 

## TLS Payload Processing 

### 1. Compression

Optionally applied must be lossless - DEFLATE

However it is vulernable due to CRIME, TIME, BREACH: Turn off TLS Compression

### 2. Hashing 

Use HMAC algorithm to compute MAC 

### 3. Encryption 

Compressed Message plus the MAC are encrypted using symmetric encryption




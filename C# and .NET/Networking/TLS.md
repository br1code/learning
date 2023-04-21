# TLS

TLS (Transport Layer Security) is a cryptographic protocol that was designed to provide secure communication over the internet. It is the successor to SSL (Secure Sockets Layer) and is now the standard protocol used for secure communication. TLS works by providing encryption and authentication of data that is transmitted over the internet.

When a TLS connection is established between two endpoints, a series of handshakes take place to set up the parameters of the connection, including the encryption algorithms and keys that will be used to encrypt the data. Once the connection is established, all data transmitted between the endpoints is encrypted using the agreed-upon encryption algorithm.

TLS provides two main types of security: encryption and authentication. Encryption ensures that the data transmitted between the two endpoints cannot be read by anyone who intercepts it. Authentication ensures that the two endpoints are who they claim to be, and not impersonators or attackers.

Here are the main steps involved in a TLS communication:

- Client Hello: The client sends a "Client Hello" message to the server, which includes the client's preferred TLS version, a random number, and a list of supported ciphers and compression methods.

- Server Hello: The server responds with a "Server Hello" message, which includes the server's preferred TLS version, a random number, and the selected cipher and compression method from the client's list.

- Certificate: The server sends its SSL/TLS certificate to the client, which includes the server's public key and other identifying information.

- Client Key Exchange: The client generates a session key, encrypts it with the server's public key from the certificate, and sends it to the server.

- Server Key Exchange: The server may send its own key exchange message if it needs to exchange additional information with the client to establish the session key.

- Certificate Request: The server may request a certificate from the client if it needs to verify the client's identity.

- Server Hello Done: The server sends a "Server Hello Done" message to indicate that the handshake is complete.

- Client Certificate: If requested, the client sends its SSL/TLS certificate to the server.

- Client Key Exchange: The client sends its session key to the server, encrypted with the server's public key.

- Certificate Verify: If requested, the client sends a digital signature of the handshake messages to the server to prove its identity.

- Change Cipher Spec: Both the client and server send a "Change Cipher Spec" message to indicate that they will start using the negotiated session key for encryption.

- Finished: Both the client and server send a "Finished" message to verify that the handshake was successful and that the session key is correctly established.

Once the TLS handshake is completed, the client and server can exchange encrypted messages using the session key. The session key is discarded when the connection is closed, and a new session key is generated for each new TLS session.

TLS is widely used in many applications, including web browsers and web servers. In fact, when you connect to a website using HTTPS (HTTP over TLS), you are using TLS to encrypt and secure the data that is transmitted between your web browser and the web server.

TLS has evolved over time, with new versions being released to address security vulnerabilities and improve performance. The most recent version is TLS 1.3, which was released in 2018 and provides improved security and faster performance than previous versions.

In summary, TLS is a protocol used to provide secure communication over the internet. It provides encryption and authentication of data transmitted between two endpoints, helping to prevent unauthorized access and eavesdropping.

Note: TLS and SSL are often used interchangeably, but they are technically different protocols. SSL is an older protocol that has been largely replaced by TLS, but the two protocols are similar in function and purpose.
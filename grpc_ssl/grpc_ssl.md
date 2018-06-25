# GRPC ssl证书生成

- 生成CA证书 
    >`复制"-----BEGIN PRIVATE KEY-----"这部分的代码到"ca.key"中`
- 1. openssl req -x509 -new -newkey rsa:1024 -nodes -out ca.pem -config ca-openssl.cnf -days 3650 -extensions v3_req
- 生成client证书
- 2. openssl genrsa -out client.key.rsa 1024
- 3. openssl pkcs8 -topk8 -in client.key.rsa -out client.key -nocrypt
- 4. rm client.key.rsa
- 5. openssl req -new -key client.key -out client.csr
- 6. openssl ca -in client.csr -out client.pem -keyfile ca.key -cert ca.pem -verbose -config openssl.cnf -days 3650 -updatedb
- 7. openssl x509 -in client.pem -out client.pem -outform PEM
- 生成server证书
- 8. openssl genrsa -out server.key.rsa 1024
- 9. openssl pkcs8 -topk8 -in server.key.rsa -out server.key -nocrypt
- 10. rm server.key.rsa
- 11. openssl req -new -key server.key -out server.csr -config server-openssl.cnf
- 12. openssl ca -in server.csr -out server.pem -keyfile ca.key -cert ca.pem -verbose -config server-openssl.cnf -days 3650 -extensions v3_req -updatedb
- 13. openssl x509 -in server.pem -out server.pem -outform PEM


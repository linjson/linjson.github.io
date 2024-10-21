# 生成本地CA根证书、p12流程

```bash
#创建根证书私钥
openssl genrsa -out root.key 2048
#创建根证书
openssl req -x509 -new -key root.key -out root.crt

#生成密码保护的私钥
openssl genpkey -algorithm RSA -out private.pem -aes256
#生成无密码的私钥
#penssl genpkey -algorithm RSA -out private.pem

#签署证书
openssl x509 -req -in certificate.csr -CA root.crt -CAkey root.key -CAcreateserial -out certificate.crt

#生成一个p12并设置密码
openssl pkcs12 -export -in certificate.crt -inkey private.pem -out certificate.p12

```
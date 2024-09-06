# 创建CA证书
```bash
# 1. 创建CA私钥 RootCaKey@2024 生成 rootca.key
openssl genpkey -aes256 -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out rootca.key -pass pass:'RootCaKey@2024'
# 2. 创建CA证书请求 生成 rootca.crt
openssl req -x509 -days 3650 -sha256 -key rootca.key -passin pass:'RootCaKey@2024' -out rootca.crt -subj "/C=CN/CN=demorootca.demo.com" -addext "subjectAltName = IP:10.9.1.68"

## 生成并导入信任证书库 PKCS12 生成 rootca.p12 TrustStore@2024
keytool -import -noprompt -trustcacerts -alias rootca -file rootca.crt -keystore rootca.p12 -storetype PKCS12 -storepass 'TrustStore@2024'
### 查看
keytool -list -v -keystore rootca.p12 -storetype PKCS12 -storepass 'TrustStore@2024'
## 生成并导入信任证书库 JKS 生成 truststore.jks
keytool -import -noprompt -trustcacerts -alias rootca -file rootca.crt -keystore truststore.jks -storetype JKS -storepass 'TrustStore@2024'
### 查看
keytool -list -v -keystore truststore.jks -storetype JKS -storepass 'TrustStore@2024'
```

# 签发服务端证书
```bash
openssl genpkey -aes256 -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out server.key -pass pass:'ServerKey@2024'
openssl req -new -key server.key -passin pass:'ServerKey@2024' -out server.csr -subj "/C=CN/CN=server.demo.com"  -addext "subjectAltName = IP:10.9.1.68"
openssl x509 -req -in server.csr -CA rootca.crt -CAkey rootca.key -passin pass:'RootCaKey@2024' -CAcreateserial -out server.crt -days 3650 -sha256
## 使用CA证书验证服务端证书
openssl verify -verbose -CAfile rootca.crt server.crt
```
# 签发客户端证书
```bash
openssl genpkey -aes256 -algorithm RSA -pkeyopt rsa_keygen_bits:4096 -out client.key -pass pass:'ClientKey@2024'
openssl req -new -key client.key -passin pass:'ClientKey@2024' -out client.csr -subj "/C=CN/CN=client.demo.com"  -addext "subjectAltName = IP:10.9.1.68"
openssl x509 -req -in client.csr -CA rootca.crt -CAkey rootca.key -passin pass:'RootCaKey@2024' -CAcreateserial -out client.crt -days 3650 -sha256
## 使用CA证书验证客户端证书
openssl verify -verbose -CAfile rootca.crt client.crt
```
# 生成PKCS12服务端证书
```bash
openssl pkcs12 -export -inkey server.key -passin pass:'ServerKey@2024' -in server.crt -chain -CAfile rootca.crt -out server.p12 -password pass:'ServerKeyStore@2024'
## 查看 server.p12证书
keytool -list -v -keystore server.p12 -storepass 'ServerKeyStore@2024'
# 生成PKCS12客户端证书
openssl pkcs12 -export -inkey client.key -passin pass:'ClientKey@2024' -in client.crt -chain -CAfile rootca.crt -out client.p12 -password pass:'ClientKeyStore@2024'
## 查看 client.p12 证书
keytool -list -v -keystore client.p12 -storepass 'ClientKeyStore@2024'
```
# 配置spring-boot工程
* application.yml
```yml
#使用rootca.p12
server:
  ssl:
    enabled: true
    key-store: classpath:cert/server.p12
    key-store-password: 'ServerKeyStore@2024'
    key-store-type: PKCS12
    key-store-provider: BC
    enabled-protocols: TLSv1.2,TLSv1.3
    trust-store: classpath:cert/rootca.p12
    trust-store-password: 'TrustStore@2024'
    trust-store-type: PKCS12
    trust-store-provider: BC
    client-auth: need

#使用 truststore.jks
server:
  ssl:
    enabled: true
    key-store: classpath:cert/server.p12
    key-store-password: 'ServerKeyStore@2024'
    key-store-type: PKCS12
    key-store-provider: BC
    enabled-protocols: TLSv1.2,TLSv1.3
    trust-store: classpath:cert/truststore.jks
    trust-store-password: 'TrustStore@2024'
    trust-store-type: JKS
    trust-store-provider: SUN
    client-auth: need

```
* 依赖
```
<dependency>
    <groupId>org.bouncycastle</groupId>
    <artifactId>bcprov-jdk18on</artifactId>
    <version>1.78.1</version>
</dependency>
```        
* 添加类
```java
static {
    Security.addProvider(new BouncyCastleProvider());
}

```
# 验证请求
```
curl -k --cert-type P12 --cert ./client.p12:'ClientKeyStore@2024' --location --request GET 'https://www.catest.com:8092/'
```



# crt转cer
openssl x509 -in server.key -in server.crt -out server.cer -outform der
openssl x509 -in rootca.key -in rootca.crt -out rootca.cer -outform der
openssl x509 -in rootca.key -in client.crt -out client.cer -outform der



openssl x509 -inform der -in server.cer -out server.pem
openssl x509 -inform der -in rootca.cer -out rootca.pem
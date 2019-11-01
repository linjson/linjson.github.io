# APK签名

```bash
    apksigner sign --ks {签名文件}
    --ks-key-alias {别名}
    --ks-pass pass:{别名密码}
    --key-pass pass:{签名文件密码} 
    --out {输出文件路径}
    {需要进行签名的压缩包}
```

# 重打包
v2打包前需要zip对齐操作
```bash
    zipalign -v 4 输入文件 输出文件
```

# 签名文件pkcs12
```bash
    keytool -importkeystore -srckeystore 输入文件 -destkeystore 输出文件 -deststoretype pkcs12
```

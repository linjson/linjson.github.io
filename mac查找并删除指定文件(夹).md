# mac查找并删除指定文件(夹)

```bash
    find . -name '<文件(夹)名称>' -exec rm -r "{}" \;
```

# mac查找文件(夹)
```bash
    find . -name '<文件(夹)名称>' -maxdepth <文件夹最大级数>
```
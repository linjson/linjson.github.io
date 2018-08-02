# jar或aar发布本地maven库

```bash
    mvn install:install-file -DgroupId=group -DartifactId=库名 -Dversion=1.0 -Dfile=文件路径 -Dpackaging=文件扩展名 -DgeneratePom=true -DlocalRepositoryPath=maven路径 -DcreateChecksum=true
```
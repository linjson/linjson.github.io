
/Users/用户名/Library/Preferences/AndroidStudio2.1/options目录下，
找到jdk.table.xml文件
```
<sourcePath>
    <root type="compostite" />
</sourcePath>
```
改为
```
<sourcePath>
    <root type="compostite" >
        <root type="simple" url="file:///路径" />
    </root>
</sourcePath>
```
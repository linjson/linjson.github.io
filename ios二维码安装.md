# ios二维码安装

1. 新建app.plist文件,内容如下
```dotnetcli
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>
            items
        </key>
        <array>
            <dict>
                <key>
                    assets
                </key>
                <array>
                    <dict>
                        <key>
                            kind
                        </key>
                        <string>
                            software-package
                        </string>
                        <key>
                            url
                        </key>
                        <string>
                            <ipa文件下载地址>
                        </string>
                    </dict>
                </array>
                <key>
                    metadata
                </key>
                <dict>
                    <key>
                        bundle-identifier
                    </key>
                    <string>
                        <app的bundleID>
                    </string>
                    <key>
                        bundle-version
                    </key>
                    <string>
                        <app的版本号>
                    </string>
                    <key>
                        kind
                    </key>
                    <string>
                        software
                    </string>
                    <key>
                        title
                    </key>
                    <string>
                        <app的名称>
                    </string>
                </dict>
            </dict>
        </array>
    </dict>
</plist>

```


2. itms-services://?action=download-manifest&url=https://<app.plist的网址> 做成二维码


3. 用iphone手机扫描这个二维码,就可以自动下载安装
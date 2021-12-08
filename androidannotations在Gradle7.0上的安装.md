# androidannotations在Gradle7.0上的安装

模块上使用androidannotations组件时,需要在`build.gradle`配置
```groovy

android {

    defaultConfig {

        javaCompileOptions {
            annotationProcessorOptions {
                arguments = [
                       "androidManifestFile": "$projectDir/src/main/AndroidManifest.xml".toString()
                ]
            }
        }
    }
}
```
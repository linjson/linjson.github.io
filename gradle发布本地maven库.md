# gradle发布本地maven库
> 创建maven.gradle文件,内容如下:


```
apply plugin: 'idea'
apply plugin: 'maven'

task androidJavadocs(type: Javadoc) {
    failOnError = false
    source = android.sourceSets.main.java.srcDirs
    ext.androidJar = "${android.sdkDirectory}/platforms/${android.compileSdkVersion}/android.jar"
    classpath += files(ext.androidJar)
}

task androidJavadocsJar(type: Jar, dependsOn: androidJavadocs) {
    classifier = 'javadoc'
    from androidJavadocs.destinationDir
}

task androidSourcesJar(type: Jar) {
    classifier = 'sources'
    from android.sourceSets.main.java.srcDirs
}

//设置pom的基本属性
group = 'xx.xx'
archivesBaseName = 'xx'
version = '1.0'


def localRepositoryUrl = "file://本地目录"

//方法一:
uploadArchives {
    repositories {
        mavenDeployer {
            snapshotRepository(url: localRepositoryUrl)
            repository(url: localRepositoryUrl) {
                
            }

        }
    }
}
//方法二:
task installArchives(type: Upload) {
    configuration = configurations.archives
    repositories.mavenDeployer {
        repository url: localRepositoryUrl
        configPom pom
    }
}

artifacts {
    //创建source的JAR包
    archives androidSourcesJar
    //创建doc的JAR包
    archives androidJavadocsJar
}

```
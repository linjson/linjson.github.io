# gradle可执行jar打包


```groovy
jar {
    String someString = ''
    manifest {
        attributes 'Main-Class': 'xx.xx'//入口类
    }
    from {
        duplicatesStrategy = DuplicatesStrategy.EXCLUDE
        configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) }
    }
}


```
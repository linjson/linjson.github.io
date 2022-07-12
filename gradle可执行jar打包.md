# gradle可执行jar打包


```groovy
  jar {
    String someString = ''
    configurations.runtime.each { someString = someString + " lib//" + it.name }
    manifest {
        attributes 'Main-Class': '执行主类'
        attributes 'Class-Path': someString
    }
    from {
        configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) }
    }
    }


```
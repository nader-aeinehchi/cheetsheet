# Scala installation
## Install Coursier CLI
```
curl -fL "https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-linux.gz" | gzip -d > cs
chmod +x cs
./cs setup
```

See https://get-coursier.io/docs/cli-installation

## Install Scala with cs
```
cs install scala:3.6.3 && cs install scalac:3.6.3
```

See https://scala-lang.org/download/3.3.4.html

## Java compatibility
https://docs.scala-lang.org/overviews/jdk-compatibility/overview.html

## SBT for Java and Scala
- compile will compile the sources under src/main/java by default
- testCompile will compile the sources under src/test/java by default

https://www.scala-sbt.org/1.x/docs/Java-Sources.html


https://get-coursier.io/docs/cli-java
```
java -version
cs java-home
cs java --installed
cs java --available

```

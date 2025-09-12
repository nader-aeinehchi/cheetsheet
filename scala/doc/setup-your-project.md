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


# Coursier
The heart of your setup is Coursier.  It manages scala, scalac, sbt etc.

To list the installed extensions/plugins, write:

```
cs list
```

You can install an extension/plugin (e.g. scalac) as:

```
cs install scalac
```

Your final setup sould look like this

```
amm
bloop
coursier
cs
metac
metap
sbt
sbtn
scala
scala-cli
scalac
scalafix
scalafmt
```



# VSCode
Vscode is preferred IDE.  ScalaIDE based on Eclipse is outdated.

In order to use Scala with Vscode, you need to install several extensions.  In particular, you need to install the following extensions:
- Scala Metals
- Extension Packe for Java.

Scala Metals is currently the preferred extension for running and debugging Scala and Java mixed environments.

# SBT
SBT is a build tool like Maven, Gradle, Pip and npm.  Use Coursier (cs) to install SBT and its clients:

```
cs install sbt
cs install sbtn 
```

In above, sbtn is the client.  You can connect to SBT server as:
```
sbt --client
```

or 
```
sbtn
```

If you want to shutdown the current SBT server, run:
```
sbt
```
and then
```
shutdown
exit
```


If you want to shudown all SBT servers, run:
```
sbt shutdownall
```

# Build server with Metals
In vscode, you can use either SBT og Bloop as build server. The documentation says that Bloop is default and has several benefits over SBT. So let it stay as Bloop (default).

I find Bloop to be problematic.  However, Bloop lets you run and debug Scala code in vscode.

You need to install additional SBT extensions as explained below:

# Scalafix SBT extension
https://scalacenter.github.io/scalafix/docs/users/installation.html#sbt

```
cs install scalafix
```

Create a new file
```
project/plugins.sbt
```

Make sure that .gitignore does not ignore the above file!

and add sbt-scalafix to it as:
```
addSbtPlugin("ch.epfl.scala" % "sbt-scalafix" % "0.14.0")
```


# Scalameta SBT extension
Install scalameta:SemanticDB is a data model for semantic information such as symbols and types about programs in Scala and other languages.  It has several plugins/extensions, see:
https://scalameta.org/docs/semanticdb/guide.html


In build.sbt add the following line:
```
libraryDependencies += "org.scalameta" %% "scalameta" % "4.12.7"
```

Note that it does not like "lastest.integration" and you need to specifically set the version number to "4.12.7"

Install and add SemanticDB (https://scalameta.org/docs/semanticdb/guide.html):
```
cs install metac metap
```

and the following lines to build.sbt
```
semanticdbEnabled := true,
semanticdbVersion := scalafixSemanticdb.revision,    
```

# Scalafix SBT extension
Scalafix is a code refactoring and linting tool that automatically fixes and improves Scala code. Here's what you need to know: What Scalafix Does:
- Spots and fixes code issues automatically
- Updates old code patterns to modern syntax
- Enforces consistent code style
- Helps with Scala version migrations

https://daily.dev/blog/scalafix-tutorial-setup-rules-configuration


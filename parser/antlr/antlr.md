# Antlr
Antlr is an old and comprehensive parser. I have not found any other good alternative parser that handles a range of different programming language:

https://www.antlr.org/about.html


# Download
Antlr has grammar files for various Java versions, including Java 20.  Download these from:

- https://www.antlr.org/download.html

# Antlr with Eclipse
Everything you need to setup Antlr for Eclipse and run code generation etc:
https://github.com/antlr4ide/antlr4ide

# Download grammar files
For Java and numerous programming languages: https://github.com/antlr/grammars-v4
https://github.com/antlr/grammars-v4/tree/master/java


# Generate lexer and parser Java code:
You can generate lexer and parser by running the grammar files.  
```
java -jar <antlr.jar> <JavaXXXLexer.g4> -o <output-folder>
java -jar <antlr.jar> <JavaXXXParser.g4> -o <output-folder>
```

e.g.

```
java -jar ~/java/antlr/antlr-4.13.2-complete.jar Java20Lexer.g4 -o generated-files/
java -jar ~/java/antlr/antlr-4.13.2-complete.jar Java20Lexer.g4 -o generated-files/
```

Having generated the lexer and parser Java code, include the generated files in the Java path. Then compile your code, typically a listener or visitor, with the generated files.
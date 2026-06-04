corrección de  linteo

en el POM 

```xml
<!-- Source: https://mvnrepository.com/artifact/com.diffplug.spotless/spotless-maven-plugin -->
<dependency>
    <groupId>com.diffplug.spotless</groupId>
    <artifactId>spotless-maven-plugin</artifactId>
    <version>3.5.1</version>
    <scope>compile</scope>
</dependency>
```
si falla en el apartado plugin:
```xml
<plugin>
    <groupId>com.diffplug.spotless</groupId>
    <artifactId>spotless-maven-plugin</artifactId>
    <version>3.6.0</version>
    <configuration>
        <java>
            <googleJavaFormat>
                <version>1.15.0</version>
            </googleJavaFormat>
        </java>
    </configuration>
</plugin>
```
y PARA APLICAR LINTEO
```sh
mvn spotless:apply
```

# Spark



1. [Create Maven Project](http://sparkjava.com/tutorials/maven-setup)
2. [Start Coding](http://sparkjava.com/documentation#getting-started)

First add spark as a dependancy in the pom.xml maven file.

```xml
<dependency>
    <groupId>com.sparkjava</groupId>
    <artifactId>spark-core</artifactId>
    <version>2.8.0</version>
</dependency>
```

Next create your main class:

```java
import static spark.Spark.*;

public class Main {
    public static void main(String[] args) {
        get("/helloworld", (req, res) -> "Hello World");
    }
}
```

You can now access the endpoint at localhost:4567/helloworld

3. Deploy to Heroku

https://sparktutorials.github.io/2015/08/24/spark-heroku.html

```xml
<build>
        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <descriptorRefs>
                        <!-- This tells Maven to include all dependencies -->
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <!--Put the name of your main class here!!-->
                        <manifest>
                            <mainClass>Main</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>

            <plugin>
                <groupId>com.heroku.sdk</groupId>
                <artifactId>heroku-maven-plugin</artifactId>
                <version>2.0.8</version>
                <configuration>
                    <jdkVersion>1.8</jdkVersion>
                    <!-- Use your own application name -->
                    <appName>mvn-test11</appName>
                    <processTypes>
                        <!-- Tell Heroku how to launch your application -->
                        <!-- You might have to remove the ./ in front   -->
                        <web>java -jar ./target/mvn-test11-1.0-jar-with-dependencies.jar</web>
                    </processTypes>
                </configuration>
            </plugin>

        </plugins>
    </build>
```



Create the heroku app:

```bash
heroku create mvn-test11
```

Then just run 

```bash
mvn heroku:deploy
```





point to godaddy subdomain:

https://www.youtube.com/watch?v=kKGSGT7mSnQ&feature=youtu.be
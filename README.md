# Maven Basics

Project Object Model (pom.xml) is a XML file that contains project information and configuration.

## Maven Phases

Name | Description
-----|------------
clean | cleans up artifacts created by prior builds
compile | compile the source code of the project
package | package it in its distributable format (jar, war, etc..)
install | install the package into the local repository (can be use as dependency locally)
deploy | done in an integration or release environment, copies the final package to the remote repository
test | test the compiled source code using a suitable unit testing framework.
test-compile |
integration-test | process and deploy the package if necessary into an environment where integration tests can be run
verify | run any checks to verify the package is valid and meets quality criteria
validate | validates the project
site | generates site documentation

## Maven Goals
Name | Description
-----|------------
dependency:copy-dependencies | opy dependencies
-DskipTests | skip test
assembly:single |

## Code Snippets

### How to add resource encoding
```xml
    <!-- project root -->
    <properties>
        <project.build.sourceEncoding>encoding</project.build.sourceEncoding>
    </properties>
```

### How to add manifest main class in pom.xml
```xml
    <!-- project root -->
    <build>
        <finalName>project_name</finalName>
        <sourceDirectory>src</sourceDirectory>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>main_class_package_path</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

## How to add manifest main class in pom.xml with included dependencies
```xml
    <!-- project root -->
    <build>
        <plugins>
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
                    <archive>
                        <manifest>
                            <mainClass>main_class_package_path</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

### How to add profiles for easy executions
```xml
    <!-- project root -->
    <profiles>
        <profile>
            <id>development</id>
            <build>
                <!-- add custom build here -->
            </build>
        </profile>
        <profile>
            <id>production</id>
            <build>
                <!-- add custom build here -->
            </build>
        </profile>
    </profiles>
```

### How to set specific resource location (can be added to profile)
```xml
    <!-- project root -->
    <build>
        <resources>
            <resource>
                <directory>resource_directory</directory>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

### How to execute Main class using mvn goal (exec:java)
```xml
    <!-- project root -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <configuration>
                    <mainClass>main_class_package_path</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

### How to bind build to execution (execute Main class with compile goal)
```xml
    <!-- project root -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.2.1</version>
                <executions>
                    <execution>
                        <!-- bind to specific goal -->
                        <phase>compile</phase> 
                        <goals>
                            <!-- may add multiple goal -->
                            <goal>java</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <mainClass>main_class_package_path</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

### How to add dependencies
```xml
    <!-- project root -->
    <dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.8.3</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

## Jar Executions

* provide class path during java execution
```shell
java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App
```

* class path is included in manifest file (for executable jar)
```shell
java -jar target/my-app-1.0-SNAPSHOT.jar
```

## References
* https://maven.apache.org/guides/
* https://maven.apache.org/plugins/index.html
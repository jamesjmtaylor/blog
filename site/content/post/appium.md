---
title: Appium
date: '2024-10-07T07:36:29-07:00'
---
![Appium AI](/img/blog/appium-sm.jpeg)

I recently had to develop a test framework for a client.  One of the requirements was that the tests had to be able to interact with elements outside of the app in order to provide a full "end-to-end" test suite.  This alone ruled out native Espresso tests, which I had used in the past, because Espresso is only capable of interacting with views within the app.

After a short stint of research I found Appium to be pretty much the industry standard for these kinds of tests.  In this post I plan to take you through the setup steps that I undertook to write and run my own Appium tests.  

The first step was to install [Appium Inspector](https://github.com/appium/appium-inspector/releases). Appium inspector provides a graphical user interface with which to record interactions and assertions with the visual elements presented on your device, including elements outside of your app's process.  This is particularly useful if your app uses an OAuth2.0 Authorization Framework.  

In order to connect to your device you'll also need to download appium, install its drivers, and run the program in your terminal.  This can be done with the following commands:

```
brew install appium &&appium driver install xcuitest &&appium driver install uiautomator2  &&appium
```

Once you have In Appium running in your terminal you will need to specify the Capabilities in the Appium Inspector's JSON Representation block. These include listing which app package to test, which activity to launch, with which automation driver, and so on.  Note that if you specify an applicationIdSuffix in your `build.gradle` you will need to exclude that suffix from the `appActivity` capability. After creating your capabilities set make sure to click the save icon.

Once the Appium Inspector session is started you can record the commands that you execute by tapping on the camera icon in the top menu.  The recorded instructions will appear in the “Recorder” tab.  If a view doesn’t reflect what the device is showing, click the “Refresh Source & Screenshot” button in the top menu.

\* In order to generate test .jar files for AWS Device Farm you will need to install selenium driver, which can be done with \`brew install selenium-server\`.  You will also need the maven build framework, which can be installed with \`brew install man\`

\* Once selenium is installed you can run \` /opt/homebrew/opt/selenium-server/bin/selenium-server standalone --port 4444\`

\* When adding Appium DesiredCapabilities to the test class that instantiates the AppiumDriver, make sure to prefix the capabilities with “appium”, i.e. \`caps.setCapability("appium:deviceName", "89SY0AWBN”);\`.  If you don’t you’ll get the error “IllegalArgumentException: Illegal key values seen in w3c capabilities“

\* If you instead start a new IntelliJ Java project, make sure to use the maven (not grade!) build system.  This is necessary because the documentation at https://docs.aws.amazon.com/devicefarm/latest/developerguide/test-types-appium-integrate.html uses pom.xml and not build.gradle.

\* When uploading to AWS Device Farm you should specifically upload `<your_project_directory_here>/target/zip-with-dependencies.zip\`.  This file is generated with the command \`mvn clean package -DskipTests=true\`

In IntelliJ you can download new maven dependencies with the hotkey CMD + Shift + I.  For an Appium test project your maven pom.xml should look something like this: 

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.evercharge</groupId>
    <artifactId>appium-tests</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    <repositories>
        <repository>
            <id>central</id>
            <url>https://repo.maven.apache.org/maven2</url>
        </repository>
    </repositories>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>5.10.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.junit.platform</groupId>
            <artifactId>junit-platform-runner</artifactId>
            <version>1.11.1</version>
        </dependency>
        <dependency>
            <groupId>io.appium</groupId>
            <artifactId>java-client</artifactId>
            <version>9.3.0</version>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.25.0</version>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
        </dependency>
        <dependency>
            <groupId>io.appium</groupId>
            <artifactId>java-client</artifactId>
            <version>9.3.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.25.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>2.6</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>test-jar</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <version>2.10</version>
                <executions>
                    <execution>
                        <id>copy-dependencies</id>
                        <phase>package</phase>
                        <goals>
                            <goal>copy-dependencies</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>${project.build.directory}/dependency-jars/</outputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>2.5.4</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                        <configuration>
                            <finalName>zip-with-dependencies</finalName>
                            <appendAssemblyId>false</appendAssemblyId>
                            <descriptors>
                                <descriptor>src/main/assembly/zip.xml</descriptor>
                            </descriptors>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

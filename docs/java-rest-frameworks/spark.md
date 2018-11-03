# Java Based REST framework evaluation

### Spark

#### Installation and setup

My first impression on going to the Spark website at http://sparkjava.com/ was good. A clearly laid out webite with a nice getting started guide.

The 'Quick Start' guide looked a little brief for me as the code shown would clearly not 
compile without somehow importing some dependencies, so I headed over to the tutorials link and followed the maven setup guide (http://sparkjava.com/tutorials/maven-setup)

This was quick and easy to follow. Soon, I had an app that I was able to run.... but how do I do that?
Of course I could have done the usual right click in IntelliJ but I wanted to run this independently of an IDE.
Using maven I generated a jar that I thought maybe would run:

```bash
mvn clean package
```

Then I tried to run this:

```bash
java -jar ./target/my-spark-app-1.0-SNAPSHOT.jar
```

I got a couple of errors here..... firstly no Main class could be found and then was missing the org.slf4j.impl.StaticLoggerBinder class.

I quickly fixed these 2 errors by adding some dependencies and 2 maven plugins - firstly the the maven-shade-plugin so that an executable jar is built
and then the maven-compiler-plugin to specify the java version.

After these steps I could generate an executable jar and run it uing the commands above. 
My first REST endpoint worked as expected, a GET to http://localhost:4567/hello.

Here is my complete working pom.xml file:

```bash
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>uk.co.cadogsoftware.spark</groupId>
    <artifactId>my-spark-app</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <dependencies>
        <dependency>
            <groupId>com.sparkjava</groupId>
            <artifactId>spark-core</artifactId>
            <version>RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.25</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.25</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>Main</mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>
```

I was frustrated that neither specifying the required dependencies or any detail of how to run the app outside of
an IDE was mentioned in the tutorial, even though these were raised as issues/questions in the comments
at the bottom of the page (http://sparkjava.com/tutorials/maven-setup)

(There is however a mention of how to build an jar with dependencies on the Heroku tutorial page here http://sparkjava.com/tutorials/heroku)


#### A simple deployment
For this I followed the Heroku tutorial here http://sparkjava.com/tutorials/heroku

I found this clear and the instructions worked without any modifications. Deploying to Heroku was quick and easy and I was quickly able to see my running app:

![My Spark app deployed to Heroku]({{ site.url }}/images/Spark_app_on_heroku.png)

NOTE: for this step I used the maven-assembly-plugin instead of the maven-shade-plugin which I used earlier to generate an executable jar.
With more time I'd nail down exactly which one of these is needed for both steps.

#### Good things
The website had good clear links to many tutorials which look to have decent content. 
Deployment to Heroku was quick and easy.

#### And the not so good
As mentioned above, the instructions given are lacking detail. Dependencies are missing and so too is
the detail of how to run your basic app outside of an IDE.

#### Overall Verdict
I don't think Spark is the worst framework I have looked at, but it's not the best either.
Although the website looks clear, some of the instructions are not complete in my opinion. 
If you are already tied into using Spark then it's probably worth continuing with it, if not then go for one of the other frameworks.

# Java Based REST framework evaluation

## Introduction

## Frameworks to evaluate (in alphabetical order)

* DropWizard
* JHipster
* RestEasy
* Spark
* SpringBoot
* Vert.x

My aim is to get a simple REST service up and running as quickly as possible in each of the above 
frameworks.

## Let's get going...


### Dropwizard

Having used Dprozizard for many years, I know how straightforward it is to get a REST service up and running with this. All the same I wanted to go right 
back to the start and try creating an app from scratch.
The Dropwizard website states that you get "Production-ready, out of the box", which sounds pretty impressive.
I followed the "Getting Started" (https://www.dropwizard.io/1.2.0/docs/getting-started.html) section and this is how I got on.

#### Installation and setup

There was no installation of any software as such. I generated a maven project using the archetype command given on the website

```bash
mvn archetype:generate -DarchetypeGroupId=io.dropwizard.archetypes -DarchetypeArtifactId=java-simple -DarchetypeVersion=1.2.0
```

As expected, this created a bare bones maven project, but it did save me a fair bit of time as a lot of the code and config was generated for me.

Next, I added a Configuration, an Application and a Representation class as described on the website.
After that it was time to add a Resource class to return a simple String in the event of a GET request to (yes, you guessed it) /hello-world.

I added a simple Healthcheck and I was good to go! 

The application started up using the command taken from the website:

```bash
java -jar target/hello-world-0.0.1-SNAPSHOT.jar server config.yml
```

The server stared up in no time and I was able to hit the simple endpoint at http://localhost:8080/hello-world. Even the option to add the version 
of your app into the Manifest file within the generated jar worked witout problem.

All of the resources served from the admin port (go to http://localhost:8081) also worked out of the box, which was great.

#### A simple deployment
I didn't try a simple deployment but is should be straightforward using the Heroku maven plugin:
https://devcenter.heroku.com/articles/deploying-java-applications-with-the-heroku-maven-plugin

#### Good things
You don't need any additional software installed in order to get going, assuming you have Java and Maven pre-installed.
It's really quick to generate a project, (especially if you use the maven archetype) and configure it in order to get something running easily.

#### And the not so good
Having to remember to add your new Resource classes to the Application run method was a bit of a pain. It would be nice if there was a 
way to do this automatically.

#### Overall Verdict
Dropwizard is a great framework to use to write REST services. All you have to do is go through the getting started page (link as above) 
in order to get going with this. I've used this framework for a number of years. It's lightweight, required little in the way of configuration 
and is easy to understand. This is a great framework.

### JHipster

JHipster was not on my original list of REST frameworks to trial, but it caught my eye when I was searching frameworks to evaluate. Here is an image of their website, outlining what you can do with it:

![What is JHipster]({{ site.url }}/images/JHipster_what_is_it.png)

Whilst this seems to be more than just a REST framework I have included this in my trial as I was excited by the possibilities this seemed to offer.

#### Installation and setup
Installation was easy – I just followed the steps on the website and managed to get a basic website backed with some REST services up and running in no time.

#### Adding Entities
I tried this in a couple of ways. Firstly, I used the online JDL Studio to firstly model my entities and then imported them into my app:

This worked without any problems. I was able to create, get, update and delete all of my new entities.

I also tried to generate simple entity from the command using a command such as this:

```bash
jhipster entity Blog
```

This worked without any problems. Again, I was able to create, get, update and delete all of my new entities.

#### A simple deployment
I deployed my JHipster app to Heroku in a matter of minutes. Assuming that you have an account and are logged into Heroku (which only takes a few minutes anyway) the deployment of my app can be done with one command:

```bash
jhipster heroku
```

One thing to note was that I used an in memory H2 database as my datastore, simply to avoid the steps needed to use something like Postgres or Mongo in Heroku, after all, I wanted to test a quick deployment.

#### How easy is it to make changes?
Once the entry in the application-dev.yml file for “livereload” had been set to true, any changes made in my IDE were picked up automatically which was good.
Adding new entities was really quick too, as detailed above, so making changes was quick and easy.

#### Limitations of my trial
I used the quickest option to get an application up and running, meaning that I went for the ‘Monolithic’ option when asked what type or application I wanted to generate. Whilst this is probably not the best option for production grade applications, it was good enough for what I wanted.
I only used simple entities with simple relationships in my evaluation. I’m sure it’s entirely possible to use much more complicated entities I didn’t try this.
I wanted a quick deployment so I used Heroku for this. I didn’t try deploying to other Cloud Platforms.

#### Good things
A whole host of stuff is available out of the box such as User Management, Auditing, Metrics, API management and multi language support.
It only takes a matter of minutes to get something up and running and deployed to Heroku.

#### And the not so good
The only criticism I can come up with, as with any generation framework, is that you don’t really get to understand the code until you make the effort to do so. A conscientious developer will, of course spend time to understand what is going on in the codebase but there is a temptation not to dig too deeply into the code and the structure of the application, which is ok …… until something needs to change or something goes wrong!

#### Overall Verdict
I was really impressed by JHipster and what is has to offer. It’s simple to generate high grade complete applications in a very short space of time. I definitely recommend taking a look at this framework.


### Resteasy

#### Installation and setup
This was a frustrating evaluation. I couldn't find any good documentation to enable me to get up and running quickly. 
Compare the Resteasy intallation and configuration page at https://docs.jboss.org/resteasy/docs/2.0.0.GA/userguide/html/Installation_Configuration.html
with the ones provided by Spring Boot or Dropwizard! The Spring and Dropwizard ones are far simpler!

Resteasy seems to be heavily tied to JBoss, and I really didn't want to be taking any time to install and configure that.
I was after a quick couple of page guide to get going and could not find one.

After some digging the best guide I could find was this one https://medium.com/@nicolaifsf78/intellij-idea-maven-resteasy-tomcat-f95bb41e6362

I followed this guide and did get to the point of generating a .war file pretty quickly, so it was time to try a deployment....

#### A simple deployment
Deployments needed a Tomcat installation so I did that easily with Homebrew

```bash
brew install tomcat
```

Then the easiest way to deploy my .war was to drop it into the tomcat 'webapps' folder (for me this is at /usr/local/Cellar/tomcat/9.0.12/libexec/webapps)
and restart tomcat using

```bash
catalina run
```

My app appeared to deploy by I got an error when trying to hit my end point.

At this point I had reached my time limit for this trial so that's where I stopped.

#### Good things
I didn't find any good things about this framework.

#### And the not so good
From what I have seen in my quick trial Resteasy does not live up to it's name! It was too cumbersome, too tied into JBoss and the documentation is poor.


#### Overall Verdict
I really wouldn't bother with this framework. I spent over 2 hours trying to get this to work and then stopped trying.
It may be pretty good if you know how to get going, but compared to Spring or Dropwizard it's not in the same league.
Only use this if you really have to!


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

### Spring Boot

#### Installation and setup
Creation of a new Spring Boot project was remarkably easy. All you need is a recent version of java (1.8+) and Maven installed locally.
I followed the 'Getting Started' guide supplied by Pivotal at https://spring.io/guides/gs/rest-service/

When going through the guide I cloned the project which contains two folders - one called 'initial' for you to use as a starting point and 
another called 'complete', which, as the name suggests contains the complete code.
After that I added the resource representation class, the resource controller and the application class.
IntelliJ provided a way to view and edit my code easily and I used Maven as my build system to generate the jar file and was able to start 
up my application using this command:

```bash
java -jar target/gs-rest-service-0.1.0.jar
```

All of this took 10-15 mins, and I was impressed by how easy this was. 

One thing I did notice from the getting started guide is that there was no mention of unit tests, but inspection of the 'complete' folder 
soon showed me how easy it was to write unit tests for my new Controller, so I copied some of these into my app.


#### Good things
I liked the way Pivotal have provided a git repo that you can clone, with two folders in it - one is an 'initial' project structure for 
you to start working on and another called 'complete' that contains the finished project.
Instructions on the website were clear and concise and I loved that it took a very small amount of time to get a basic REST service up and running.
Personally, I love the Spring annotations and how concise they enable the code base to be.

#### And the not so good
A bit more information on the unit testing side of things would have been good. I'm sure this is somewhere in the Spring documentation but it would have 
been good to talk a bit about unit testing in the 'getting started' guide.

#### Overall Verdict
As expected, this is a really easy way to get up and running. You hardly need to write any code to create REST services. 
This is one of my favourite frameworks in this evaluation. If you are already using Spring for other things then this is the framework 
for you. Even if you are not using Spring, then you really need to consider Spring Boot as it's really good.


### Vert.x

#### Installation and setup
From reading the introduction at https://vertx.io/ it was clear that this framework offers much more than
just a REST framework, but for the purposes of this evaluation I wanted to stick to trying to create a basic service.
I wasn't 100% sure where to start if I'm honest, so I went for the 'Starter' link at the top of the main
page (http://start.vertx.io/), filled in the details and downloaded my first project.

I then imported this into my IDE to take a look at what had been generated.

As it turns out this generated just what I wanted - a simple service that I could spin up and hit right away.

One of the concepts Vert.x uses is called 'Verticles'. This is what was generated for me.
Although I didn't find the code as intuitive to follow as some of the other frameworks it was 
obvious what the 'Verticle' was doing.

Following the 'README' in the generated project enabled me to package my project using the usual command:

(NOTE: this link also looks like a good place to start https://github.com/vert-x3/vertx-examples/tree/master/maven-simplest)

```bash
./mvnw clean package
```

Unfortunately, the instructions to run the project were wrong. This command was given:

```bash
./mvnw clean exec:java
```

but if I followed this I got an error:

```bash
java.lang.ClassNotFoundException: io.vertx.starter.MainVerticle
```
which was sort of expected as the 'clean' phase gets rid of the generated jar. Using this command fixed the issue:

```bash
./mvnw clean package exec:java
```

Now I had a simple server running and going to http://localhost:8080 gave me the good old Hello World text in return

![Vert.x hello world]({{ site.url }}/images/Vertx_hello_world.png)


#### Good things
You can use Vert.x with multiple languages including Java, JavaScript, Groovy, Ruby, Ceylon, Scala and Kotlin
Eclipse Vert.x is event driven and non blocking

#### And the not so good
Lots of features (which is good!) means it may be hard to find what you are looking for in the documentation.

#### Overall Verdict
Although I did find creating a REST endpoint easy, I didn't find it as 
intuitive as some of the other frameworks. 
The documentation was pretty clear but as Vert.x is clearly much more than a REST it was harder to find the information 
I needed really quickly. 
I certainly think more time is required to evaluate the other areas of the framework.
If you are looking for a framework that does a whole lot more than just REST endpoints then
Vert.x is worth looking at in more detail than I have here.


### Conclusion

I have quickly investigated several java REST frameworks. My aim was to get something running quickly in each of these 
so I limited myself to a small amount of time on each.
Here is how I rank the frameworks in order of best to worst:

1. SpringBoot
2. Dropwizard
3. JHipster
4. Vert.x
5. Spark
6. RestEasy







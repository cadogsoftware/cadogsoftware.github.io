# Java Based REST framework evaluation

Note : this page is in progress. I'll update it as more frameworks are evaluated.

## Introduction


## Frameworks to evaluate

* DropWizard
* JHipster
* SpringBoot
* RestEasy
* Spark
* Vertex

## Let's get going...

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

#### A simple deployment
TODO

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

TODO

### Vertex

TODO

### Conclusion
TOTO




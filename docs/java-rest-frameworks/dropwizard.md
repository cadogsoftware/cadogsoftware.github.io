# Java Based REST framework evaluation

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

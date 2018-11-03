# Java Based REST framework evaluation

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


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

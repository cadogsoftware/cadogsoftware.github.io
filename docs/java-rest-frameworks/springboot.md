# Java Based REST framework evaluation

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


# Java Based REST framework evaluation

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

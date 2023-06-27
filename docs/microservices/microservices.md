# Microservices - the good and the bad

## What is a Microservice
It's difficult to find a consistent definition of what a microservice is but one of our favourites is from https://microservices.io/

"Microservices - also known as the microservice architecture - is an architectural style that structures an application as a collection of services that are

Highly maintainable and testable
Loosely coupled
Independently deployable
Organized around business capabilities
Owned by a small team
The microservice architecture enables the rapid, frequent and reliable delivery of large, complex applications. It also enables an organization to evolve its technology stack."

## Why use Microservices
Let's be clear here, we are fans of splitting out functionality into distinct areas of functionality and getting away from the monolith approach, but done badly microservices can be massively complex and a nightmare to maintain.

On the other hand, microservices done well can be a pleasure to work with and provide real flexibility and scalability to your organisation. Areas of business functionality can be worked on independently and changes can go from the mind of the Product Owner to the live environment in a matter of minutes.
Contrast this with the monolith approach used by so many organisations in the past, where changes in one area of code have knock on effects to many other (often understood) areas of the system. Testing is painful, the release process is slow and dealing with multiple versions is almost impossible.
So we recommend using microservices, but make sure to avoid the pitfalls below and you will have a rewarding experience.

## Bad things we have seen when working with MS

### No common definitions
How you define terms within your business organisation and software teams must be consistent. You must make sure that when a developer is talking about something, everyone else on the team thinks of this as the same thing. For example we have worked on teams where the developer would refer to a user's "role" when determining what resources they could access, but the Product Owner would refer to the user's "permissions and policies". Immediately this caused confusion and led to wasted time and bugs until a single version of terms used within the system (also called a ubiquitous common language) was authored and then used by all members of the team.

### When microservices become too 'micro'
One client who was migrating from a monolith took the microservices approach too far in our opinion. There were six or seven separately deployable Spring Boot Java services to handle what was straightforward alerting framework. Each of the services did a similar thing but had to have it's own deployable module created along with interfaces defined. At runtime messages had to be send over the network bwtween the modules, a common code library had to be created in an attempt to not duplicate code, but worst of all, when a change was needed, be it in the infrastructure code or the module's functionality, changes had to be made across all modules. Trying to get all modules running locally and communicating in the correct way was a challenge in itself!
This orgainisation would have been far better off keeping all of the alerting related code in one module that exposed clearly defined endpoints. Reusable code could have been kept within the module, no messages would have be be sent over the network and running this locally would have been easy.
This is a reminder that you can go too far with microservices, but creating the correct balance of splitting functionality up and keeping business concepts  together is key - time should be taken to define the 'bounded context' (see below for what this means!) for your services before writing too much code. Also don't be afraid to change and improve as services and functionality are extended.

### No bounded context

### No testing from the start TDD and BDD


### Lack of clean interfaces and documentation
### No Product Owner engagement


## How to do Microservices well

Deliver work from the start
Common library and versioning
Version things properly
Test test test
Automated tests in th CI/CD pipeline
Good testing e.g. unit testing should do just that
Spread the knowledge but have specialists too (but not just one)
Pick a good infrastructure
Add healthchecks
Open API (Swagger)



## Other things
logging accross the end to end system

## Useful resources

https://microservices.io/

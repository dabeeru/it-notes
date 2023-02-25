---
tags:
  - IT
  - Architecture
	- Microservices
---
Still read those chapters
- Chapter 5 Splitting the Monolith
- Chapter 10 Conways Law and System Design
- Chapter 12 Bringing It All Together

# Benefits
## Technology heterogenity
- Microservices allow us to more easily experiment with different solution and technologies and let our system to evolve more easily
- Trade off of using many different technologies and programming languages should be still taken into account though

## Resilience
- In microservice architecture in general it might be easier to pin down the root cause of  the issue in the system (at least to specific microservice) and protect other services for cascading the failure

## Scalling 
- Microservices allows us to scale different component of the application independently
- Scaling in times of tool like [[Kubernetes]] and [[Cloud]] is much easier than in the past

## Ease of deployment 
- Every microservice should be deployable without need to deploy anything else
- Possibility of deploy one microservices independently (so single application component) greately decrease the risk of single release (comparing to monolithic application) 
- In result microserices can be deployed to PROD much more often which decrease risk of big outgage (like in big release)
- Small release makes the investigation of the PROD issue much easier (since scope of the change is limited) and also simplify and speeding up the rollback

## Organizational Alignement
- Smaller teams working on smaller codebases tend to be more productive

## Composability
- It's still about abstraction - services should deliberetly expose some functionalities while hide internal implementation so other services wouldn't be able tou couple to much to its internals

## Replacibility
- Cost of replacing small microservices is much lower than in case of monolithic application

## Security
- Distributed systems by definition are characterized with high level of segragation
- If implemented according to [[Least Privilage Principle]], in case of one microservice would be compromise it doesn't automatically compromise the whole system

# Architecture
### Conaiderations
- Every check in is release candidate
- Microservice boundaries should match service boundaries
- Rule of a thumb: Microservice is a piece of software that can be rewriten in 2 weeks
- It's always about some trade offs, most basic trade off in Microservice architecture is that such finely grained systems will always have more independce regarding specific components but the complexity of system in overall also increases
- In distributed microservices based systems architect should be rather considering the big picture rather than specific solution within single microservice
- Also general health of the whole system should be much more improtant than single microservice
- Even though microservices gives us flexibilty regarding what technology is used in every service, it might be wise to impose some catalogue of allowed technologies - it might be hard to hire people and support the system which is to much heterogenous
- We should also agree on integration method which all microservices should use (eg. [[REST]], [[RPC]]) - shouln't use more than 2 methods
- It's higly advised that architect should spend time closely with development team and get his hand dirty to understand technology stack deeply
- Share capabilities not data
- Distributed system should be focused on providing business capabilities, it shouldn't be just bunch os not related CRUD services 
- Do not violate [[DRY]] withing the service but be relaxed regarding violating it between services - loosing loose coupling is usually much worse than duplicating behaviours

## Loose coupling
- Change in single microservice should almost never mean the necessity of change in another services 
- Every microservice should know as little as necessary to collaborate with other services
- Number of calls between microservices should be limited to avoid chatty communication which might lead to tight coupling 

## High Cohesion
- All closely related behaviours should be placed withing single microservice
- So if there is change of the specific element of the business process then it should impact only one microservice responsible for related functionality

## Bounded context
- Every domain can be divided into multiple bounded contexts
- Each contents is build with some internal models which shouldn't be exposed outside the context and internal models which are meant to be exchanged in other bounded contexts
- Every microservice should represent one bounded context which ensures high cohesion and reduce coupling between microservices
- Microservice should expose API including models which are meant to be shared with external world, and hide rest in its internal implementation
- Every bigger bounded context can be divided into multiple nested contexts, it needs to be decided if microservice boundaries  should be laid on the broaded boundary or on nested one. Very often decision can be made based upon the organizational structure - can single bounded context be handled by single team should we divide it to multiple ones. Just have cohesion in mind. 

## Versioning 
- Best way to handle breaking changes is not to make them in the first place

## Integration
- API should be technology-agnostic
- API should be as simple as possible 
- Internal representation should be hidden behind the API, avoid patterns like sharing single database schema between multiple microservices - it would lead to loosing cohesion and increase coupling
- Most commonly [[HTTP]] is used us underlying protocol (eg. [[SOAP]], [[REST]]), but some [[RPC]] technologies use binary data exchange on top of plain [[TCP]]
- [[Websockets]] can be also used to stream some data over plain [[TCP]]
- Keep middleware dumb, think twice before introducing heavy [[Event Bus]] instead of simpler [[message broker]] (eg. [[RabbitMQ]], [[Kafka]], [[ActiveMQ]])

###  RPC
**[[RPC]]** - Remote Procedure Call - is the pattern of integration where the local procedure call on one machine can result in code execution on the another.

- Most of those technology are exchanging binary data but some like [[SOAP]] are using XML and [[HTTP]].
- Also API should be designed with caution - with [[RPC]] it's very tempting to treat such remote invocations as local one which might lead to very chatty communiaction between services which could impact system performance.
- It's also easier to introduce higher coupling between services, specialy in context of exchanged model, some [[RPC]]  may require to rebuild client stub even when removing the functionality which hasn't been used by client service.
- [[RPC]] might be also using [[UDP]] as underlying protocol which might be good fit for high performance, low latency systems

There are different implementation of this pattern:
- Technology agnostic
	- eg. [[SOAP]], [[Thrift]], [[Protocol Buffers]]
	- relying on external interface definition (eg. [[SOAP]] Interface, or [[WSDL]] (Web Service Definitinion Language)) which allows to generate client and servers stubs in different programming languages 
- Technoogy specific - eg. [[Java RMI]]

### REST
- [[REST]] is build around Resources
- [[REST]] is most comonly used via [[HTTP]]
	- [[REST]] relies on [[HTTP]] like POST, PUT, GET, DELETE
- [[HTTP]] based [[REST]] can be quite easily cached with various [[Reverse Proxy]] solutions (eg. [[Varnish, [[mod_proxy]])

#### Client libraries
- It's possible to write or automatically generate client libraries that client can use to communicate with REST API via HTTP
- This might bring some benefit but in most cases usage of proper HTTP client should be enough

### Hypermedia
- There are some Hypermedia based technologies for integration like [[HATEOAS]] (*Hypermedia as the engine of application state*)
- This technology highly relies on links which can be used to interact with the server to change the resource state
- This technology offers possibilty of very loosed decoupling 
- The trade off is that communication here can be quite chatty

### JSON vs XML
- [[XML]] has much more capabilites
	- links between resources
- [[JSON]] is most commonly used recently
- There are [[JSON]] extensions like [[HAL]] which are meant to address lack of hyperlinks support
- For both there are some simple query languages like [[JSONPATH]] and [[XPATH]]


## Deployment 
- With usage of [[RPC]] as integration method, automation and ease of process of generating client and server stubs is crucial for usage of this technology, otherwise using it might be quite cumbersome.

## Synchronicity
- Distributed system can be used in two different style:
	- synchronous request/response orchestration 
	- asynchronous event based choreography

### Synchronous 
- Synchronous aproach assumes that connection between client and serven is maintained open until the requested action is fullfiled
- Eventually we can still get some level of anychronitcity while relying on synchronous call by registering the callbacks

#### Orchestration
- This approach leads keeping the core logic of the process in one place - service acting as orchestrator which calls other services to request some sub-actions - this pattern is called Orchestration
- This approach is leading to have some god services containing highlevel logic telling other CRUD services how to handle data. 
- Orchestration based systems are simpler in small scale but might not scales as well as Choreography based systems

### Asynchronous 
- Aynchronous approach can be good in systems which include long running jobs and in general can improve system characteristics regarding latency but it increases system's general complexity
- Fully aynchronous approach assumes having some kind of an event bus where some services can publish  events and other can subscribe to it

#### Choreography
- This way the logic of the whole process is distributed across the system - there is no central place where the whole process is codified, single services are just doing their part, produces events based on their actions and trust that other services will do their part - this is called Choreography
- Choregraphy based systems are decoupled by design 
- Downside of choregraphy approach is that business process are not reflected in single services but rather in whole system which makes its harder to monitor and debug
- Choreography based systems are much better suited for big distibuted systems

# Architect's mindset
Making architectural decisions is always matter of making some trade offs, we can think of following framework for making such decisions properly - this top down approach help us think more strategically about certain decision to make:

### Strategic goals 
- strategic goals made be the business organization itself (eg. expanding to certaing market, maximizing conversion of leads, maximizing time of application usage)

### Principles 
- high level rules which has to be followed to achive some larger goal, eg.
- All logs should be gather centrally
- Development team should have ownership of the service from beginning of the implementation up to code deployment
- they might be either some hard constraints which cannot be workaround due to limitation of technology or rules which are more good practices (we might want to distinguish them though)
- example might be [Heroku 12 Factors rulset](https://12factor.net/)

### Practices
- Practices are there to ensure that principles are really implemented
- Should be much more detailed and low-level than Principles
- Examples:
	- Coding guidlines
	- Observability guidlines
	- Rules regarding integration
	- Deployment rules (eg. every service needs to be deployed on separate environment)
 
For small teams combining practices and principles might be fine, but in larger organization we might need to thing about how level principles which are for all the teams and some sets of concrete practices which applies to different set of teams which for example differ regarding to used technology stack.

### Technical debt
- Technical debt can be added when cutting corners in order to achieve some short-term goal.
- Acquiring technical debts means paying ongoing cost and additional work in the future to pay it down. 
- If we would change the overall characteristics of the system all outdated components can be also treated as technical debt
- We should keep on eye on the technical debt in our system and decide when and to what extent we should pay it back

### Standarization
- It's important to define common attributes that each microservice should have - this way we can have some level of standarization and sharing good practices between microservices still having the service-to-service autonomy autonomy
- [[Observability]] is one of the topics that needs to be standarized between services to ensure that it would be possible to continuously asses the health of the service
- Standards should also touch resiliency topics like having dedicating [[service connection pool]] and using [[Circut Breaker]]
- There should be shared service template (or templates in case of multiple technology stacks) or even microservice skaffolding tool which could be use to easily create new services and addopt all common practices and translate local discoveries into global practices
- Such template should be maintained be developers themselves
- Some things which are not meant to change very offten can be also captured by shared libraries - eg. logging, http client

### Exceptions
- It might be necessary to make some exception in to the desired standards
- Often if the same exceptions are being made then it means that standard should be extended or changed to reflect this new way of working 

### Governance
- Governance is process of ensuring that the build system is matching its architectural vision
- It should be group excesise, the group should be led by the technologist and consist sinior members of teams which are responsible for delivering the system
- If group disagrees with architect decision, it might be ofter right - group of people is usually smarter than simple person

# Other decomposition techniques
## Caution 
- True technology heterogenicity is being lost
- Change in the library might require redeployment of all of the services
- Shared code can became a place of coupling of many microservices so it's reducing cohesion of the system
- Scope of shared libraries (and other decomposition techniques) should be minimal and lited to things which are common for all services and not meant to change very often 
- Consider the risk of introducing unnecesarry coupling and think of propagating common things through service template or skaffolding tools  

## Middleware functions
- Simple libraries which can be used as middleware for handling the request eg. obfuscating payloads, loggin request related inormation etc. 
- Should be quite easy to maintain stable API since it should simply pass through request object

## Shared libraries
- Common code can be abstracted to some shared libraries
- Those libraries can be reused in different services
- The same teams which are responsible for the microservices should maintain the shared libraries
- Think twice about introducing the common shared library for things like entity models shared between multiple microservices

## Modules
- In some programming languages there are some modules, wchich can be deployed into running process without restarting it (effecitvely, releasing new version of the module shouldn't mean releasing whole application).
- [[OSGI]] - Open Source Service Initiative - is JVM related technology which allows such capabilities
	- In general it's considered to be an easy source of increased complexity and hard technology to maintain
- [[Erlang]] also support modules interactively
- Modules just like shared libraries are not considered to introduce much a gain in regards to distributed system design comparing to microservices, most importnantly they are introducing a dangerous point of coupling


# Error handling

### Mapping errors for upstream service failure

Errors codes should be always considered from the perspective of the client, so if the request from the client was legit, and error is caused purely because the reason that is between our service and upstream service it should be always mapped to 5XX error.  It might be 500 or 502, although 502 might be better reserved for components like API Gateways, It might be 503 in case that we are expecting the issue to be temporary. 
